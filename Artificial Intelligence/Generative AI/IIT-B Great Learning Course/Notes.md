K-fold cross-validation = better estimate of how well the model will perform on unseen data.

---

Stratified = preserve class proportions

So when you see:

> Imbalanced classification + Cross-validation

Think:

> Stratified k-Fold

---

Grid Search = try everything → thorough but expensive.

---

Imbalanced data → don't trust Accuracy alone.

---

Precision means:

> Of everything the model predicted as positive, how many were actually positive?

Precision → Predicted Positive

Metric Breakdown
- TP (True Positive): Correct positive predictions.
- FP (False Positive): Incorrect positive predictions.
- FN (False Negative): Incorrect negative predictions.
- TN (True Negative): Correct negative predictions.

Other Formulas for Context
- Recall: TP / (TP + FN)
- Specificity: TN / (TN + FP)
- Accuracy: (TP + TN) / (TP + TN + FP + FN)

---

High Recall → minimize False Negatives → don't miss positive cases.

High Precision → minimize False Positives → don't raise too many false alarms.

---

