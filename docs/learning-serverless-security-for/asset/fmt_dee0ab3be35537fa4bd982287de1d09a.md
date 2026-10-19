
A fully managed service for storing, managing, and securing container images and other artifacts in Google Cloud. By storing container images in Artifact Registry, you can version and manage images that Cloud Run services use for deployment.

### Note

This section assumes you are using your Serverless Lab account, which hosts the vulnerable-by-design serverless lab environment.

Let’s walk through the steps to set up the Cloud Run service along with its associated service account:

1. Run the following commands to clone the `cloud-run-html-to-pdf` repository:
```
cd ~

USERNAME=learning-serverless-security
REPO=cloud-run-html-to-pdf

git clone https://github.com/$USERNAME/$REPO.git

cd cloud-run-html-to-pdf
```

2. Examine the source code of `main.py` (inside `cloud-run-html-to-pdf`):
```
cat main.py
```
Running this command displays the application’s source code in `main.py`, similar to the following:
```
import os
import subprocess
from flask import Flask, request

...

@app.route("/", methods=["POST"])
def handle_gcs_event():
    event = request.get_json()
    print(event)

    ...

    print("Generate PDF using wkhtmltopdf")
    try:
        subprocess.run(f"wkhtmltopdf {local_input_path} ...", shell=True)
        print(f"Generated PDF at {local_output_path}")
    except subprocess.CalledProcessError as e:
        print(f"wkhtmltopdf error: {e}")
        return "PDF generation failed", 500

    ...

    return f"Successfully processed {filename}", 200

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=8080)
```
This application handles Cloud Storage events by downloading the uploaded file via `gsutil`, converting it to a PDF with `wkhtmltopdf`, and uploading the generated PDF to the designated output bucket. However, the code executes `wkhtmltopdf` as a shell command using the untrusted filename value from the event, introducing a code injection vulnerability that could allow arbitrary command execution.

#### Note

Feel free to review the application’s implementation in the [`cloud-run-html-to-pdf` GitHub repo](https://oreil.ly/tOzLs).

3. Remove the local Git repository metadata to detach the project from version control:
```
rm -rf .git
```

4. Ensure that the required Google Cloud services for the serverless lab are enabled:
```
gcloud services enable run.googleapis.com \
    storage.googleapis.com \
    eventarc.googleapis.com \
    cloudbuild.googleapis.com
```

5. Initialize variables for your project ID, region, and service account email:
```
PROJECT_ID=$(gcloud config get-value project)
REGION="us-east1"
REPO_NAME="learning-serverless-security"
SERVICE_NAME="html-to-pdf-service"
SA_NAME="html-to-pdf"
SA_EMAIL="$SA_NAME@$PROJECT_ID.iam.gserviceaccount.com"
```

6. Create a new service account named `html-to-pdf-service`:
```
gcloud iam service-accounts create $SA_NAME \
  --display-name="Cloud Run HTML to PDF Service Account"
```

7. Grant the service account `html-to-pdf-service` owner permissions on the project:
```
gcloud projects add-iam-policy-binding $PROJECT_ID \
  --member="serviceAccount:$SA_EMAIL" \
  --role="roles/owner"
```

#### Note

If you encounter `INVALID_ARGUMENT: Service account html-to-pdf@…​.iam.gserviceaccount.com does not exist`, wait a few minutes for the service account to propagate and then retry the command.

8. Create an Artifact Registry repository:
```
gcloud artifacts repositories create $REPO_NAME \
  --repository-format=docker \
  --location=$REGION \
  --project=$PROJECT_ID \
  --description="Learning Serverless Security Resources Repository"
```

9. Build and push the container image for your Cloud Run service to Google Artifact Registry:
```
TAG="$REGION-docker.pkg.dev/$PROJECT_ID/$REPO_NAME/$SERVICE_NAME"

gcloud builds submit --tag $TAG
```
Alternatively, you can skip building locally by pulling an existing image from Docker Hub and pushing it to Artifact Registry:
```
TAG="$REGION-docker.pkg.dev/$PROJECT_ID/$REPO_NAME/$SERVICE_NAME"

docker pull learningserverlesssecurity/cloud-run-html-to-pdf:1

docker tag learningserverlesssecurity/cloud-run-html-to-pdf:1 $TAG

docker push $TAG
```

#### Caution

This container image is vulnerable by design and intended for demonstrating serverless security issues in a lab environment. Do not use it for production workloads.

10. Verify that the `OUTPUT_BUCKET` environment variable is correctly set before deploying the service:
```
source ~/bucket-vars.sh

[[ -z "$OUTPUT_BUCKET" ]] && echo "Error: OUTPUT_BUCKET is not set."
```

11. Deploy the Cloud Run service:
```
ENV_VARS="OUTPUT_BUCKET=$OUTPUT_BUCKET,SECRET=PASSWORD123"

gcloud run deploy $SERVICE_NAME \
  --image $TAG \
  --region $REGION \
  --no-allow-unauthenticated \
  --service-account=$SA_EMAIL \
  --set-env-vars $ENV_VARS \
  --max-instances=1 \
  --concurrency=1 \
  --timeout=1200
```
The following table lists each of the configuration options used:

| Flag/option | Description | Value |
| --- | --- | --- |
| `--image` | Specifies the container image to be used for the Cloud Run service deployment | The container image you pushed to Artifact Registry in an earlier step |
| `--region` | Sets the Google Cloud region where the service will be deployed | `us-east1` |
| `--no-allow-unauthenticated` | Ensures that only authenticated requests can access the service | Configuration prevents the service from being accessed by public/unauthenticated HTTP requests |
| `--service-account` | Assigns a service account that the service will use to access other resources | `html-to-pdf@…​.⁠iam​.gserviceaccount​.com` |
| `--set-env-vars` | Sets environment variables for the service | `OUTPUT_BUCKET` and `SECRET` |
| `--max-instances` | Limits the maximum number of container instances for the service | `1` |
| `--concurrency` | Sets the number of requests a single container instance can handle concurrently | `1` |
| `--timeout` | Defines the maximum request duration before the request is terminated | 20 minutes |

12. Check the details of your deployed Cloud Run service, including its configuration and status:
```
gcloud run services describe $SERVICE_NAME \
  --region $REGION
```
The full configuration details are returned, similar to the following:
```
✔ Service html-to-pdf-service in region us-east1

URL:     https://html-to-pdf-service-....us-east1.run.app
Ingress: all
Traffic:
  100% LATEST (currently html-to-pdf-service-...)

Scaling: Auto (Min: 0)

Last updated on 2025-00-00T00:00:00.000000Z by ...:
  Revision html-to-pdf-service-...
  Container None
    Image:           .../html-to-pdf-service
    Port:            8080
    Memory:          512Mi
    CPU:             1000m
    Env vars:
      OUTPUT_BUCKET  ss-output-bucket-...
      SECRET         PASSWORD123
    Startup Probe:
      TCP every 240s
      Port:          8080
      Initial delay: 0s
      Timeout:       240s
      Failure threshold: 1
      Type:          Default
  Service account:   html-to-pdf@....iam.gserviceaccount.com
  Concurrency:       1
  Max instances:     1
  Timeout:           1200s
```
