Cancer Detection via Gene Expression
K-Nearest Neighbors Classification
Can two gene expression measurements reliably predict cancer presence? This project applies K-Nearest Neighbors classification to a gene expression dataset, progressing from a naive k=1 baseline through cross-validated hyperparameter tuning to identify the optimal neighborhood size.

Overview
KNN is a distance-based algorithm that classifies a new observation by majority vote among its k nearest training neighbors. Because it relies on Euclidean distance, feature scaling is critical — unscaled features with larger ranges will dominate the distance calculation regardless of their predictive value.
This project demonstrates the full KNN workflow: exploratory visualization, proper scaling inside a Pipeline to prevent data leakage during cross-validation, elbow method analysis, and GridSearchCV to select the optimal k value.

Dataset
The gene expression dataset contains measurements of two gene activity levels (Gene One and Gene Two) across patients, with a binary label indicating cancer presence (1) or absence (0). The two-feature structure allows the decision boundary to be fully visualized.
FeatureDescriptionGene OneExpression level of first geneGene TwoExpression level of second geneCancer PresentBinary target: 1 = cancer detected, 0 = not detected

Workflow
1. Exploratory Data Analysis
Scatter plots of gene expression levels colored by cancer class confirm the classes are not perfectly linearly separable, motivating a non-parametric classifier like KNN.
2. Baseline Model (k=1)
An initial model with k=1 classifies each point based solely on its nearest neighbor. This tends to overfit, memorizing training noise rather than learning generalizable patterns. The model achieves high training accuracy but is expected to underperform on unseen data.
3. Elbow Method
Sweeping k from 1 to 29 and plotting test error rate at each step reveals a natural elbow around k=14, where error stabilizes. Values beyond this add neighbors without meaningfully improving accuracy.
4. Pipeline and GridSearchCV
To evaluate k properly with cross-validation, a Pipeline chains StandardScaler and KNeighborsClassifier. This ensures scaling is refit inside each fold, preventing any test-set information from leaking into the scaler during the grid search. A 5-fold GridSearchCV across k values 1 through 19 confirms k=14 as optimal.
5. Final Model Evaluation
The tuned pipeline (k=14) is fit on the training set and evaluated on the held-out test set. Performance is reported via accuracy, confusion matrix, and a full classification report including precision, recall, and F1 per class.
6. Single-Sample Inference
The final pipeline demonstrates real-time prediction on a single observation, returning both the predicted class and class probability estimates.

Results
ModelAccuracyKNN (k=1, baseline)~94%KNN (k=14, tuned)~95%
The tuned model improves generalization over the k=1 baseline by reducing variance from overfitting to individual training points.

Key Concepts
Curse of Dimensionality: KNN degrades in high-dimensional spaces as distances become less meaningful. This dataset's two-feature structure keeps the algorithm effective.
Pipeline for Leak-Free Cross-Validation: Fitting the scaler inside the pipeline ensures the test fold is never seen during scaling — a critical detail when using GridSearchCV.
Elbow Method vs. GridSearchCV: Both approaches converge on k=14, validating each other. The elbow method gives a quick visual heuristic; GridSearchCV provides a rigorous cross-validated selection.

Stack

Python 3
Pandas, NumPy
Matplotlib, Seaborn
scikit-learn (KNeighborsClassifier, Pipeline, GridSearchCV, StandardScaler)
