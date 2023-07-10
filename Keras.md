
# Keras Cheatsheet

## Model building

### Architecture specification

~~~

import numpy as np
from tensorflow.keras.layers import Dense
from tensorflow.keras.models import Sequential

data = np.loadtxt('data.csv', delimiter=',')
n_cols = data.shape[1]

model = Sequential()

# Input layer
model.add(Dense(100, activation='relu', input_shape = (n_cols,)))

# Hidden layer
model.add(Dense(100, activation='relu'))

# Output layer
model.add(Dense(1))

# Print out model architecture
print(model.summary())

~~~

### Compiling model

~~~

model.compile(optimizer='adam', loss='mean_squared_error')

~~~

### Fitting model

~~~

model.fit(data, target)

~~~

## Problem types

### Regression

- Predicting outcomes based on historical continuous data.
- Typically use "mean_squared_error" for loss function.

### Classification

- Predicting outcomes from a set of discrete options.
- Typically use "categorical_crossentropy" for loss function.
- Add "metrics = ['accuracy']" to see progress at each epoch.
- Output layer has a separate node for each possible outcome, and uses the "softmax" activation function.

### Example classification code

~~~

from tensorflow.keras.utils import to_categorical

data = pd.read_csv('basketball_shot_log.csv')
predictors = data.drop(['shot_result'], axis=1).values
target = to_categorical(data['shot_result'])

model = Sequential()
model.add(Dense(100, activation='relu', input_shape = (n_cols,)))
model.add(Dense(100, activation='relu'))
model.add(Dense(100, activation='relu'))
model.add(Dense(2, activation='softmax'))
model.compile(optimizer='adam', loss='categorical_crossentropy', metrics=['accuracy'])
model.fit(predictors, target)

~~~

## Reusing models

~~~

from tensorflow.keras.models import load_model

model.save('my_model.h5')
my_model = load_model('my_model.h5')
predictions = my_model.predict(test_data)
probability_true = predictions[:,1]

~~~
