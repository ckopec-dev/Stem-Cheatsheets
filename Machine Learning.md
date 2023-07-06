
# Machine Learning Cheatsheet

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

## Unsupervised learning

Similar to supervised, except the data has no targets. Useful for:

- Anomaly detection
- Diving data into groups (a.k.a. clustering)

## Workflow

- Starts with historical/existing data
- Extract features
- Split data into training set and test set
- Select and train a model with the training set
- Evaluate the model's performance with the test set
- If the performance is not acceptable, tune it and retrain
- Repeat until performance is acceptable. If it never is, it might be due to inadequate data.
