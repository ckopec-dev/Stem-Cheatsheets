
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

~~~

  
