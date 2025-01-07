
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

### Classification

~~~
from sklearn.neighbors import KNeighborsClassifier

# Features
x = churn_df[["total_day_charge", "total_eve_charge"]].values

# Target (observations)
y = churn_df["churn"].values

# Create classifier and fit it
knn = KNeighborsClassifier(n_neighbors=15)
knn.fit(x, y)

# 3 new observations to predict (same number of columns as features)
x_new = np.array([[12, 13], [4, 2], [4, 6]])
predictions = knn.predict(x_new)

# Show predictions (a binary value for each observation, i.e. the predicted target)
print("Predictions: {}".format(predictions))
~~~

### Measuring performance

~~~
from sklearn.model_selection import train_test_split

# Split data into train and test sets
x_train, x_test, y_train, y_test = train_test_split(x, y, test_size=0.3, random_state=21, stratify=y)

# Create classifier and fit it
knn = KNeighborsClassifier(n_neighbors=6)
knn.fit(x_train, y_train)

# Check accuracy. Score returns a decimal number from 0 to 1.
print(knn.score(x_test, y_test))
~~~
