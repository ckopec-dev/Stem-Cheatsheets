<!-- markdownlint-disable MD024 -->

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

## TensorFlow Keras Tutorial - Complete Beginner's Guide

### Table of Contents

1. [What is TensorFlow and Keras?](#what-is-tensorflow-and-keras)
2. [Installation](#installation)
3. [Basic Concepts](#basic-concepts)
4. [Your First Neural Network](#your-first-neural-network)
5. [Working with Different Data Types](#working-with-different-data-types)
6. [Model Training and Evaluation](#model-training-and-evaluation)
7. [Advanced Topics](#advanced-topics)
8. [Best Practices](#best-practices)

### What is TensorFlow and Keras?

**TensorFlow** is an open-source machine learning framework developed by Google. It's designed for building and deploying machine learning models at scale.

**Keras** is a high-level neural networks API that runs on top of TensorFlow. It makes building deep learning models simple and intuitive.

#### Key Features

- **User-friendly**: Simple, consistent interface
- **Modular**: Easy to configure neural networks
- **Extensible**: Easy to add new modules
- **Python-based**: Leverages Python's simplicity

### Installation

```bash
# Install TensorFlow (includes Keras)
pip install tensorflow

# For GPU support (optional)
pip install tensorflow-gpu

# Verify installation
python -c "import tensorflow as tf; print(tf.__version__)"
```

## Basic Concepts

#### 1. Tensors

Tensors are multi-dimensional arrays - the fundamental data structure in TensorFlow.

```python
import tensorflow as tf
import numpy as np

# Creating tensors
scalar = tf.constant(42)                    # 0D tensor (scalar)
vector = tf.constant([1, 2, 3, 4])         # 1D tensor (vector)
matrix = tf.constant([[1, 2], [3, 4]])     # 2D tensor (matrix)
tensor_3d = tf.constant([[[1, 2]], [[3, 4]]])  # 3D tensor

print(f"Scalar shape: {scalar.shape}")
print(f"Vector shape: {vector.shape}")
print(f"Matrix shape: {matrix.shape}")
```

#### 2. Keras Layers
Layers are the building blocks of neural networks.

```python
from tensorflow import keras
from tensorflow.keras import layers

# Common layer types
dense_layer = layers.Dense(64, activation='relu')  # Fully connected layer
conv_layer = layers.Conv2D(32, (3, 3), activation='relu')  # Convolutional layer
dropout_layer = layers.Dropout(0.5)  # Regularization layer
```

#### 3. Models
Models define the architecture of your neural network.

```python
# Sequential model (linear stack of layers)
model = keras.Sequential([
    layers.Dense(64, activation='relu'),
    layers.Dense(32, activation='relu'),
    layers.Dense(10, activation='softmax')
])

# Functional API (more flexible)
inputs = keras.Input(shape=(784,))
x = layers.Dense(64, activation='relu')(inputs)
x = layers.Dense(32, activation='relu')(x)
outputs = layers.Dense(10, activation='softmax')(x)
model = keras.Model(inputs=inputs, outputs=outputs)
```

### Your First Neural Network

Let's build a simple neural network to classify handwritten digits using the MNIST dataset.

```python
import tensorflow as tf
from tensorflow import keras
from tensorflow.keras import layers
import matplotlib.pyplot as plt

# 1. Load and prepare the data
(x_train, y_train), (x_test, y_test) = keras.datasets.mnist.load_data()

# Normalize pixel values to 0-1 range
x_train = x_train.astype('float32') / 255.0
x_test = x_test.astype('float32') / 255.0

# Reshape data (flatten 28x28 images to 784-dimensional vectors)
x_train = x_train.reshape(x_train.shape[0], -1)
x_test = x_test.reshape(x_test.shape[0], -1)

print(f"Training data shape: {x_train.shape}")
print(f"Training labels shape: {y_train.shape}")

# 2. Build the model
model = keras.Sequential([
    layers.Dense(128, activation='relu', input_shape=(784,)),
    layers.Dropout(0.2),
    layers.Dense(64, activation='relu'),
    layers.Dropout(0.2),
    layers.Dense(10, activation='softmax')  # 10 classes (digits 0-9)
])

# 3. Compile the model
model.compile(
    optimizer='adam',
    loss='sparse_categorical_crossentropy',
    metrics=['accuracy']
)

# View model architecture
model.summary()

# 4. Train the model
history = model.fit(
    x_train, y_train,
    batch_size=32,
    epochs=10,
    validation_split=0.1,
    verbose=1
)

# 5. Evaluate the model
test_loss, test_accuracy = model.evaluate(x_test, y_test, verbose=0)
print(f"Test accuracy: {test_accuracy:.4f}")

# 6. Make predictions
predictions = model.predict(x_test[:5])
predicted_classes = tf.argmax(predictions, axis=1)
print(f"Predicted classes: {predicted_classes}")
print(f"Actual classes: {y_test[:5]}")
```

### Working with Different Data Types

#### Image Classification (CNN)

```python
# For image data, use Convolutional Neural Networks
def create_cnn_model(input_shape, num_classes):
    model = keras.Sequential([
        layers.Conv2D(32, (3, 3), activation='relu', input_shape=input_shape),
        layers.MaxPooling2D((2, 2)),
        layers.Conv2D(64, (3, 3), activation='relu'),
        layers.MaxPooling2D((2, 2)),
        layers.Conv2D(64, (3, 3), activation='relu'),
        layers.Flatten(),
        layers.Dense(64, activation='relu'),
        layers.Dense(num_classes, activation='softmax')
    ])
    return model

# For CIFAR-10 dataset
(x_train, y_train), (x_test, y_test) = keras.datasets.cifar10.load_data()
x_train, x_test = x_train / 255.0, x_test / 255.0

cnn_model = create_cnn_model((32, 32, 3), 10)
cnn_model.compile(
    optimizer='adam',
    loss='sparse_categorical_crossentropy',
    metrics=['accuracy']
)
```

#### Text Classification (RNN/LSTM)

```python
# For sequential data like text
def create_text_model(vocab_size, max_length, embedding_dim=100):
    model = keras.Sequential([
        layers.Embedding(vocab_size, embedding_dim, input_length=max_length),
        layers.LSTM(64, dropout=0.5, recurrent_dropout=0.5),
        layers.Dense(1, activation='sigmoid')
    ])
    return model

# Example with IMDB movie reviews
vocab_size = 10000
max_length = 500

text_model = create_text_model(vocab_size, max_length)
text_model.compile(
    optimizer='adam',
    loss='binary_crossentropy',
    metrics=['accuracy']
)
```

### Model Training and Evaluation

#### Training with Callbacks

```python
# Define callbacks for better training control
callbacks = [
    keras.callbacks.EarlyStopping(
        monitor='val_loss',
        patience=3,
        restore_best_weights=True
    ),
    keras.callbacks.ReduceLROnPlateau(
        monitor='val_loss',
        factor=0.2,
        patience=2,
        min_lr=0.001
    ),
    keras.callbacks.ModelCheckpoint(
        'best_model.h5',
        monitor='val_accuracy',
        save_best_only=True
    )
]

# Train with callbacks
history = model.fit(
    x_train, y_train,
    batch_size=32,
    epochs=50,
    validation_split=0.2,
    callbacks=callbacks,
    verbose=1
)
```

#### Visualizing Training History

```python
def plot_training_history(history):
    fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(12, 4))
    
    # Plot training & validation accuracy
    ax1.plot(history.history['accuracy'], label='Training Accuracy')
    ax1.plot(history.history['val_accuracy'], label='Validation Accuracy')
    ax1.set_title('Model Accuracy')
    ax1.set_xlabel('Epoch')
    ax1.set_ylabel('Accuracy')
    ax1.legend()
    
    # Plot training & validation loss
    ax2.plot(history.history['loss'], label='Training Loss')
    ax2.plot(history.history['val_loss'], label='Validation Loss')
    ax2.set_title('Model Loss')
    ax2.set_xlabel('Epoch')
    ax2.set_ylabel('Loss')
    ax2.legend()
    
    plt.tight_layout()
    plt.show()

# Use the function
plot_training_history(history)
```

#### Model Evaluation

```python
# Detailed evaluation
from sklearn.metrics import classification_report, confusion_matrix
import seaborn as sns

# Get predictions
y_pred = model.predict(x_test)
y_pred_classes = tf.argmax(y_pred, axis=1)

# Classification report
print(classification_report(y_test, y_pred_classes))

# Confusion matrix
cm = confusion_matrix(y_test, y_pred_classes)
plt.figure(figsize=(10, 8))
sns.heatmap(cm, annot=True, fmt='d', cmap='Blues')
plt.title('Confusion Matrix')
plt.ylabel('Actual')
plt.xlabel('Predicted')
plt.show()
```

### Advanced Topics

#### Custom Layers

```python
class CustomDenseLayer(layers.Layer):
    def __init__(self, units, activation=None):
        super(CustomDenseLayer, self).__init__()
        self.units = units
        self.activation = keras.activations.get(activation)
    
    def build(self, input_shape):
        self.w = self.add_weight(
            shape=(input_shape[-1], self.units),
            initializer='random_normal',
            trainable=True
        )
        self.b = self.add_weight(
            shape=(self.units,),
            initializer='zeros',
            trainable=True
        )
    
    def call(self, inputs):
        output = tf.matmul(inputs, self.w) + self.b
        if self.activation is not None:
            output = self.activation(output)
        return output
```

#### Transfer Learning

```python
# Use pre-trained models
base_model = keras.applications.VGG16(
    weights='imagenet',
    include_top=False,
    input_shape=(224, 224, 3)
)

# Freeze base model weights
base_model.trainable = False

# Add custom classification head
model = keras.Sequential([
    base_model,
    layers.GlobalAveragePooling2D(),
    layers.Dense(128, activation='relu'),
    layers.Dropout(0.2),
    layers.Dense(num_classes, activation='softmax')
])
```

#### Data Augmentation

```python
# Create data augmentation pipeline
data_augmentation = keras.Sequential([
    layers.RandomFlip("horizontal"),
    layers.RandomRotation(0.1),
    layers.RandomZoom(0.1),
    layers.RandomContrast(0.1),
])

# Add to model
model = keras.Sequential([
    data_augmentation,
    layers.Conv2D(32, 3, activation='relu'),
    # ... rest of model
])
```

### Best Practices

#### 1. Data Preparation

- **Normalize your data**: Scale features to similar ranges
- **Handle missing values**: Impute or remove missing data
- **Split data properly**: Use train/validation/test splits
- **Data augmentation**: Increase dataset size artificially

#### 2. Model Architecture

- **Start simple**: Begin with simple models, add complexity gradually
- **Use appropriate activation functions**: ReLU for hidden layers, softmax for multi-class
- **Add regularization**: Use dropout, batch normalization
- **Choose right optimizer**: Adam is often a good default

#### 3. Training

- **Monitor overfitting**: Use validation data and early stopping
- **Use callbacks**: EarlyStopping, ReduceLROnPlateau, ModelCheckpoint
- **Experiment with batch sizes**: Larger batches = more stable gradients
- **Learning rate scheduling**: Reduce learning rate during training

#### 4. Evaluation

- **Use multiple metrics**: Don't rely on accuracy alone
- **Cross-validation**: For smaller datasets
- **Test on unseen data**: Final evaluation on test set
- **Visualize results**: Confusion matrices, training curves

#### 5. Model Deployment

```python
# Save model
model.save('my_model.h5')

# Load model
loaded_model = keras.models.load_model('my_model.h5')

# Save weights only
model.save_weights('model_weights')

# Load weights
model.load_weights('model_weights')

# Convert to TensorFlow Lite for mobile
converter = tf.lite.TFLiteConverter.from_keras_model(model)
tflite_model = converter.convert()
```

### Next Steps

1. **Practice with real datasets**: Kaggle competitions, UCI ML repository
2. **Learn specialized architectures**: ResNet, BERT, Transformer
3. **Explore TensorFlow Extended (TFX)**: For production ML pipelines
4. **Study MLOps**: Model versioning, monitoring, A/B testing
5. **Join the community**: TensorFlow forums, GitHub, Stack Overflow

### Resources

- **Official Documentation**: https://www.tensorflow.org/guide
- **Keras Documentation**: https://keras.io/
- **TensorFlow Tutorials**: https://www.tensorflow.org/tutorials
- **Deep Learning Specialization**: Coursera
- **Books**: "Deep Learning with Python" by François Chollet

Happy learning! 🚀
