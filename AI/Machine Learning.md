
# Machine Learning Cheatsheet

## Workflow

- Starts with historical/existing data
- Extract features
- Split data into training set and test set
- Select and train a model with the training set
- Evaluate the model's performance with the test set
- If the performance is not acceptable, tune it and retrain
- Repeat until performance is acceptable. If it never is, it might be due to inadequate data.
  
## Supervised learning

Starts with training data. Data contains:

- Target variables: the columns that we want to predict
- Labels: the values of the target variable rows
- Features: the columns that the model learns from and uses to create predictions
- Examples: feature + target data

Example:

| FeatureA | FeatureB | TargetA |
| -------- | -------- | ------- |
| Data1    | Data2    | Label   |

### Classification

Assigning a category as an answer to a question. E.g. What kind of iris is that?

- Linear classifier: separates data into categories with a straight line
- Polynomial classifier: separates data into categories with a curve

### Regression

Assigns a continuous variable. E.g. What will this stock be worth?

## Unsupervised learning

Similar to supervised, except the data has no targets. Useful for:

### Clustering

Divides data into groups with similar characteristics

### Anomaly detection

Detecting outliers. Data points that strongly differ from the others. Useful in fraud detection.

### Association

Finding relationships between observations. E.g. which objects are bought together.

## Evaluating classification models

- Overfitting: this is when the model performs great on training data but poorly on test data.
- Accuracy: correctly classified observations / all observations. Not very useful when there are few labels of a certain type.
- Confusion matrix: a better way of showing accuracy. Results are displayed in a grid such as:  

| | | Actual | Actual |
| - | - | - | - |
| | | Y | N |
| Predicted | Y | true positives | false positives |
| Predicted | N | false negatives | true negatives |

- Sensitivity: true positives / (true positives + false negatives)
- Specificity: true negatives / (true negatives + false positives)

## Evaluating regression models

- Error: distance between actual value and predicted value

## Tuning models

### Dimensionality reduction

Removing features that may be irrelevant.  
Some features might be highly correlated, and therefore redundant.

### Hyperparameter tuning

Tweaking the algorithm's parameters. Differs by algorithm.

### Ensemble methods

Combines several models to produce a single optimal model.
With regression, the predictions from multiple models are typically averaged.

## Deep learning

- Subset of Machine Learning
- Uses neural networks to emulate animal brains
- Requires large amounts of data
- Useful when input is images or text

## Business applications

- Draw causal insights. E.g. why are customers leaving?
- Predict future events. E.g. which customers are likely to leave?
- Understand patterns in data. E.g. are there groups of similar customers?

### Examples by industry

- Marketing
  - Predict which customers will purchase next month
  - Predict each customer's lifetime value
  - Group customers into segments based on past purchases
- Finance
  - Predict fraud
  - Predict loan payments
  - Group transactions based on risk
- Manufacturing
  - Robotics
  - Predict which production items are faulty
  - Predict which machines need maintenance
  - Group sensor readings and identify anomalies
- Transportation
  - Predict delivery times
  - Predict product demand
  - Identify optimal routing
