# TensorFlow Keras Cheatsheet

## Import and Setup

```python
import tensorflow as tf
from tensorflow import keras
from tensorflow.keras import layers
import numpy as np
import matplotlib.pyplot as plt

# Check TensorFlow version
print(tf.__version__)

# Set random seed for reproducibility
tf.random.set_seed(42)
```

## Model Creation

### Sequential Model
```python
model = keras.Sequential([
    layers.Dense(64, activation='relu', input_shape=(784,)),
    layers.Dropout(0.2),
    layers.Dense(10, activation='softmax')
])

# Alternative syntax
model = keras.Sequential()
model.add(layers.Dense(64, activation='relu', input_shape=(784,)))
model.add(layers.Dropout(0.2))
model.add(layers.Dense(10, activation='softmax'))
```

### Functional API
```python
inputs = keras.Input(shape=(784,))
x = layers.Dense(64, activation='relu')(inputs)
x = layers.Dropout(0.2)(x)
outputs = layers.Dense(10, activation='softmax')(x)
model = keras.Model(inputs=inputs, outputs=outputs)
```

### Subclassing API
```python
class MyModel(keras.Model):
    def __init__(self):
        super(MyModel, self).__init__()
        self.dense1 = layers.Dense(64, activation='relu')
        self.dropout = layers.Dropout(0.2)
        self.dense2 = layers.Dense(10, activation='softmax')
    
    def call(self, inputs):
        x = self.dense1(inputs)
        x = self.dropout(x)
        return self.dense2(x)

model = MyModel()
```

## Common Layers

### Dense Layers
```python
layers.Dense(units=64, activation='relu')
layers.Dense(10, activation='softmax')
layers.Dense(1, activation='sigmoid')
```

### Convolutional Layers
```python
layers.Conv2D(32, (3, 3), activation='relu')
layers.Conv1D(64, 3, activation='relu')
layers.MaxPooling2D((2, 2))
layers.GlobalAveragePooling2D()
```

### Recurrent Layers
```python
layers.LSTM(50, return_sequences=True)
layers.GRU(32)
layers.SimpleRNN(64)
layers.Bidirectional(layers.LSTM(50))
```

### Regularization Layers
```python
layers.Dropout(0.5)
layers.BatchNormalization()
layers.LayerNormalization()
```

### Embedding and Preprocessing
```python
layers.Embedding(vocab_size, embedding_dim)
layers.TextVectorization(max_tokens=10000)
layers.Normalization()
layers.Rescaling(1./255)
```

## Activation Functions

```python
# Common activations
'relu', 'sigmoid', 'tanh', 'softmax', 'softplus'
'swish', 'gelu', 'elu', 'selu', 'linear'

# Custom activation
layers.Dense(64, activation=tf.nn.relu)
layers.Activation('relu')
```

## Model Compilation

```python
model.compile(
    optimizer='adam',
    loss='sparse_categorical_crossentropy',
    metrics=['accuracy']
)

# Custom optimizer
model.compile(
    optimizer=keras.optimizers.Adam(learning_rate=0.001),
    loss=keras.losses.SparseCategoricalCrossentropy(),
    metrics=[keras.metrics.SparseCategoricalAccuracy()]
)
```

### Common Optimizers
```python
keras.optimizers.Adam(learning_rate=0.001)
keras.optimizers.SGD(learning_rate=0.01, momentum=0.9)
keras.optimizers.RMSprop(learning_rate=0.001)
keras.optimizers.AdamW(learning_rate=0.001, weight_decay=0.01)
```

### Common Loss Functions
```python
# Classification
'binary_crossentropy'
'categorical_crossentropy'
'sparse_categorical_crossentropy'

# Regression
'mse' or 'mean_squared_error'
'mae' or 'mean_absolute_error'
'huber'
```

## Training

### Basic Training
```python
history = model.fit(
    x_train, y_train,
    epochs=10,
    batch_size=32,
    validation_data=(x_val, y_val),
    verbose=1
)
```

### With Callbacks
```python
callbacks = [
    keras.callbacks.EarlyStopping(patience=3, restore_best_weights=True),
    keras.callbacks.ModelCheckpoint('best_model.h5', save_best_only=True),
    keras.callbacks.ReduceLROnPlateau(factor=0.2, patience=2),
    keras.callbacks.TensorBoard(log_dir='logs')
]

history = model.fit(
    x_train, y_train,
    epochs=100,
    validation_data=(x_val, y_val),
    callbacks=callbacks
)
```

## Data Preprocessing

### Image Data
```python
# Using tf.data
dataset = tf.data.Dataset.from_tensor_slices((x_train, y_train))
dataset = dataset.batch(32).prefetch(tf.data.AUTOTUNE)

# Data augmentation
data_augmentation = keras.Sequential([
    layers.RandomFlip("horizontal"),
    layers.RandomRotation(0.1),
    layers.RandomZoom(0.1),
])
```

### Text Data
```python
# Tokenization
tokenizer = keras.preprocessing.text.Tokenizer(num_words=10000)
tokenizer.fit_on_texts(texts)
sequences = tokenizer.texts_to_sequences(texts)

# Padding
padded = keras.preprocessing.sequence.pad_sequences(sequences, maxlen=100)
```

## Model Evaluation and Prediction

```python
# Evaluation
loss, accuracy = model.evaluate(x_test, y_test, verbose=0)

# Prediction
predictions = model.predict(x_test)
class_predictions = np.argmax(predictions, axis=1)

# Single prediction
single_pred = model.predict(np.expand_dims(x_single, axis=0))
```

## Saving and Loading Models

```python
# Save entire model
model.save('my_model.h5')
model.save('my_model')  # SavedModel format

# Load model
loaded_model = keras.models.load_model('my_model.h5')

# Save/load weights only
model.save_weights('model_weights.h5')
model.load_weights('model_weights.h5')

# Save/load architecture
model_json = model.to_json()
with open('model.json', 'w') as f:
    f.write(model_json)
```

## Custom Components

### Custom Layer
```python
class CustomLayer(layers.Layer):
    def __init__(self, units):
        super(CustomLayer, self).__init__()
        self.units = units
    
    def build(self, input_shape):
        self.w = self.add_weight(
            shape=(input_shape[-1], self.units),
            initializer='random_normal'
        )
    
    def call(self, inputs):
        return tf.matmul(inputs, self.w)
```

### Custom Loss Function
```python
def custom_loss(y_true, y_pred):
    return tf.reduce_mean(tf.square(y_true - y_pred))

model.compile(optimizer='adam', loss=custom_loss)
```

### Custom Metric
```python
def custom_accuracy(y_true, y_pred):
    return tf.reduce_mean(tf.cast(tf.equal(y_true, tf.round(y_pred)), tf.float32))
```

## Transfer Learning

```python
# Load pre-trained model
base_model = keras.applications.VGG16(
    weights='imagenet',
    include_top=False,
    input_shape=(224, 224, 3)
)

# Freeze base model
base_model.trainable = False

# Add custom layers
model = keras.Sequential([
    base_model,
    layers.GlobalAveragePooling2D(),
    layers.Dense(128, activation='relu'),
    layers.Dense(num_classes, activation='softmax')
])
```

## GPU and Performance

```python
# Check GPU availability
print("GPU Available: ", tf.config.list_physical_devices('GPU'))

# Mixed precision training
policy = tf.keras.mixed_precision.Policy('mixed_float16')
tf.keras.mixed_precision.set_global_policy(policy)

# Distributed training
strategy = tf.distribute.MirroredStrategy()
with strategy.scope():
    model = create_model()
    model.compile(...)
```

## Debugging and Visualization

```python
# Model summary
model.summary()

# Plot model architecture
keras.utils.plot_model(model, show_shapes=True, show_layer_names=True)

# View training history
plt.plot(history.history['loss'])
plt.plot(history.history['val_loss'])
plt.legend(['train', 'validation'])
plt.show()

# TensorBoard
tensorboard_callback = keras.callbacks.TensorBoard(log_dir="logs")
```

## Common Model Architectures

### CNN for Image Classification
```python
model = keras.Sequential([
    layers.Conv2D(32, (3, 3), activation='relu', input_shape=(28, 28, 1)),
    layers.MaxPooling2D((2, 2)),
    layers.Conv2D(64, (3, 3), activation='relu'),
    layers.MaxPooling2D((2, 2)),
    layers.Conv2D(64, (3, 3), activation='relu'),
    layers.Flatten(),
    layers.Dense(64, activation='relu'),
    layers.Dense(10, activation='softmax')
])
```

### LSTM for Sequence Classification
```python
model = keras.Sequential([
    layers.Embedding(vocab_size, 64),
    layers.LSTM(64, dropout=0.5, recurrent_dropout=0.5),
    layers.Dense(1, activation='sigmoid')
])
```

### Autoencoder
```python
# Encoder
encoder = keras.Sequential([
    layers.Dense(128, activation='relu', input_shape=(784,)),
    layers.Dense(64, activation='relu'),
    layers.Dense(32, activation='relu')
])

# Decoder
decoder = keras.Sequential([
    layers.Dense(64, activation='relu', input_shape=(32,)),
    layers.Dense(128, activation='relu'),
    layers.Dense(784, activation='sigmoid')
])

# Autoencoder
autoencoder = keras.Sequential([encoder, decoder])
```
