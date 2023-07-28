
# Scikit-learn Cheatsheet

## Supervised learning

- The predicted values are known.
- The goal is to predict target values of unseen data, given the features.
  - Features are also known as predictor variables and independent variables.
  - The target variable is also known as the response variable and dependent variable.

### Types of supervised learning

- Classification: target value consists of categories
- Regression: target value is continuous

### Data requirements

- No missing values
- Data must be in numeric format
- Data must be stored in a pandas DataFrame or NumPy array

### General syntax/workflow

~~~

from sklearn.module import Model

model = Model()
model.fit(x, y)
predictions = model.predict(new_observations_to_predict)
print(predicitions)

~~~
