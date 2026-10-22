# redaction with the Google Cloud DLP API

```
dlp_output = stt_nlp_output | 'RedactTextOptional'
>> beam.Map(lambda j: redact_text(j, project_id))
```

The Framework also uses the higher-level Map transform as seen in the code snippet below, Figure 4-5 represents the visual transformation in the Dataflow interface and Figure 4-6 and 4-7 represents the input and output elements of the transformation.

```
# function to redact sensitive data if dlp key value is set to true
def redact_text(data, project):
    info_types = []
    deidentify_config = {
        "info_type_transformations": {
            "transformations": [
                {
                    "primitive_transformation": {
                        "replace_config": {
                            "new_value": {
                                "string_value": '#',
                            }
                        }
                    }
                }
            ]
        }
    }
    if data['dlp'] == 'true' or data['dlp'] == 'True':
        dlp = google.cloud.dlp_v2.DlpServiceClient()
        parent = dlp.project_path(project)
        response = dlp.list_info_types('en-US')
        for info_type in response.info_types:
            info_types.append({'name': info_type.name})
        inspect_config = { "info_types": info_types}
        item = {"value": data['transcript']}
        response = dlp.deidentify_content(
            parent,
            inspect_config=inspect_config,
            deidentify_config=deidentify_config,
```