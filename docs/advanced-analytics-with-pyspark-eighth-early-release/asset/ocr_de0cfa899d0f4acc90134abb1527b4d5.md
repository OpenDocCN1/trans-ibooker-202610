```
evaluator =
MulticlassClassificationEvaluator(labelCol="Cover_Type",
                  predictionCol="prediction")
accuracy =
evaluator.setMetricName ("accuracy").evaluate(pred_df)
f1 = evaluator.setMetricName ("f1").evaluate(pred_df)
mlflow.log_metrics ({"accuracy": accuracy, "f1": f1})
# Log artifact: feature importance scores
tree_model = pipeline_model.stages[-1]
feature_importance_df =
(pd.DataFrame(list(zip(vector_assembler.getInputCols (),
                      tree_model.featureImportances)),
             columns=["feature", "importance"])
     .sort_values (by="importance", ascending=False))
feature_importance_df.to_csv ("feature-importance.csv",
               index=False)
mlflow.log_artifact ("feature-importance.csv")
```

We can now access our experiment data via the tracking UI. Start it by
running `mlflow ui` command. It starts by default on port 5000. You can
use the `-p <port_name>` option to change the default port. Once you
have successfully started the UI, go to _http://localhost:5000/_. You will see a
UI as shown in Figure 11-3. You can search across all the runs, filter for
those that meet particular criteria, compare runs side by side, etc. If you
wish, you can also export the contents as a CSV file to analyze locally.
Click on the run in the UI named "decision-tree".