A confusion matrix shows how often each class was correctly/falsely classified. Each entry describes the count.

|                 | Pedestrians | Non-pedestrians | Total Instances |
| --------------- | ----------- | --------------- | --------------- |
| Pedestrians     | TP 800      | FN 100          | P 900           |
| Non-pedestrians | FP 5        | TN 95           | N 100           |

From the confusion matrix you can calculate different statistics about the classification model:
$$\text{Accuracy } a=\frac{TP+TN}{n}$$
$$\text{Precision } p = \frac{TP}{TP+FP}$$
$$\text{Recall (True postive rate) } r = \frac{TP}{TP+FN}$$
$$\text{False positive rate} = \frac{FP}{FP+TN}$$