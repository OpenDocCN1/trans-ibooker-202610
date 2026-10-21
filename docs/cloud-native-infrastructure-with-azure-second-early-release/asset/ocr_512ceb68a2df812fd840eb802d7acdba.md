+   - Select how you would like to open your project: choose “Add to workspace”.
+   - Using this information, Visual Studio Code generates an Azure Functions project with an HTTP trigger. You can view the local project files in the Explorer. To learn more about files that are created, see the Generated project files documentation.

+   - At this time, you’ll have the skeleton of a project created. Add the following code to the *HttpExample* file, which will return the IP of the client making a request to the function (similar to *icanhazip.com*):

```
import azure.functions as func

def main(req: func.HttpRequest ) -> func.HttpResponse:
    response = False
    if (req.headers.get('X-Forwarded-For')):
        response = req.headers.get('X-Forwarded-For').split(':')[0]
    else:
        response = "127.0.0.1"
    return func.HttpResponse(response)
```

+   - Test your function by pressing F5 on your keyboard and then going to http://localhost:7071/api/HttpExample.
+   - To deploy to Azure, you will need to click the “Sign in to Azure…” button in the Azure Functions window.
+   - Click the Upload button, which will allow you to upload your function to the app you created previously.

Now that we’ve looked at the Azure native offerings, let’s discuss the function apps deployed with Kubernetes.