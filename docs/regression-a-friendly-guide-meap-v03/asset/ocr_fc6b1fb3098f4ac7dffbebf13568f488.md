true positive rate versus the false positive rate as the threshold varies; for historical reasons, this graph is known as the *ROC curve*, where *ROC* stands for *Receiver Operating Characteristic*. To create the ROC curve for binary classifiers based on our NAMCS logistic model, we could continue creating confusion matrices and then plot the resulting rates, putting the false positive rate (i.e., 1 - specificity) on the horizontal axis and the true positive rate (the sensitivity) on the vertical axis. Summarizing what we’ve done so far, we know three points on the ROC curve:

| Threshold | False positive rate | True positive rate |
|---|---|---|
| 20% | 0.735 | 0.975 |
| 50% | 0.377 | 0.832 |
| 80% | 0.09 | 0.349 |

While we could write a simple script to determine the remainder of the ROC curve, it’s much easier to use the roc_curve function from the sklearn metrics module. What’s more, we can use this module’s auc function to compute the *area under the curve (AUC)*, an important metric of overall performance. Here are the details:

```
fpr, tpr, thresholds = metrics.roc_curve(chronic.Condition, chron
chronic_auc = metrics.auc(fpr, tpr)
chronic_auc

0.7938824622209679

plt.figure()
plt.plot(fpr, tpr, label='ROC curve      AUC: %0.2f' % chronic_auc)
plt.plot([0,1], [0,1], 'r--', label='Random classification')
plt.plot([0.735,0.377,0.09], [0.975,0.832,0.349], 'ok')
plt.xlim([0, 1])
plt.ylim([0, 1.05])
plt.xlabel('False Positive Rate (1-Specificity)')
plt.ylabel('True Positive Rate (Sensitivity)')
```