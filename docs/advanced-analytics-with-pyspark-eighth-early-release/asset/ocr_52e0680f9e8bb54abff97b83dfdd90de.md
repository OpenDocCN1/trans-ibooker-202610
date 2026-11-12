```
artifact_path: model
flavors:
python_function:
data: sparkml
env: conda.yaml
loader_module: mlflow.spark
python_version: 3.6.12
spark:
model_data: sparkml
pyspark_version: 3.0.1
run_id: 0433bb047f514e28a73109bbab767222
utc_time_created: '2021-11-06 12:34:27.758915'
```

Figure 11-5. MLflow Model

The file captures our model's metadata, signature and flavors. The Model signature defines the schema of a model's inputs and outputs.

Our model file has two flavors: python_function and spark. The python_function flavor enables MLflow's model deployment and serving tools to work with any Python model regardless of which ML library trained the model. As a result, any Python model can be easily productionalized in a variety of runtime environments.

The spark model flavor enables exporting Spark MLlib models as MLflow Models. For example, to make predictions on a Spark DataFrame using the loggel model:

```
import mlflow

run_id = "0433bb047f514e28a73109bbab767222" 1
logged_model = f'runs:/{run_id}/model' 2

# Load model as a Spark UDF.
loaded_model = mlflow.spark.load_model(model_uri=logged_model)

# Predict on a Spark DataFrame.
preds = loaded_model.transform(test_data)
preds.select('Cover_Type', 'rawPrediction', 'probability',
'prediction').show(1, vertical=True)
...
-RECORD 0----------------------------------------
```