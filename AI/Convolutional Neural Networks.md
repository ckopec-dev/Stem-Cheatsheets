
# 🧠 TensorFlow Convolutional Neural Network (CNN) Tutorial

---

## 1. What Is a CNN?

A **Convolutional Neural Network** is a deep learning model designed for **image and pattern recognition**.
It uses **convolutional layers** to automatically extract spatial features from images — things like edges, textures, and shapes — and learns hierarchical representations.

### CNN Building Blocks:

| Layer Type       | Description                                     |
| ---------------- | ----------------------------------------------- |
| **Conv2D**       | Extracts local features using filters (kernels) |
| **MaxPooling2D** | Reduces spatial size, keeps key features        |
| **Flatten**      | Converts 2D feature maps into 1D vectors        |
| **Dense**        | Fully connected layers for classification       |
| **Dropout**      | Reduces overfitting                             |

---

## 2. Install and Import Dependencies

```bash
pip install tensorflow
```

Then in Python:

```python
import tensorflow as tf
from tensorflow.keras import datasets, layers, models
import matplotlib.pyplot as plt
```

---

## 3. Load the Dataset (CIFAR-10)

CIFAR-10 contains 60,000 color images (32×32 pixels) in 10 classes (e.g., airplane, cat, ship, truck).

```python
(x_train, y_train), (x_test, y_test) = datasets.cifar10.load_data()
x_train, x_test = x_train / 255.0, x_test / 255.0

# Class names for visualization
class_names = ['airplane', 'automobile', 'bird', 'cat', 'deer',
               'dog', 'frog', 'horse', 'ship', 'truck']
```

---

## 4. Visualize the Data

```python
plt.figure(figsize=(10,10))
for i in range(25):
    plt.subplot(5,5,i+1)
    plt.xticks([]); plt.yticks([])
    plt.imshow(x_train[i])
    plt.xlabel(class_names[int(y_train[i])])
plt.show()
```

---

## 5. Build the CNN Model

Here’s a **basic CNN** architecture:

```python
model = models.Sequential([
    # First convolutional block
    layers.Conv2D(32, (3,3), activation='relu', input_shape=(32, 32, 3)),
    layers.MaxPooling2D((2,2)),

    # Second block
    layers.Conv2D(64, (3,3), activation='relu'),
    layers.MaxPooling2D((2,2)),

    # Third block
    layers.Conv2D(64, (3,3), activation='relu'),

    # Flatten + Dense layers
    layers.Flatten(),
    layers.Dense(64, activation='relu'),
    layers.Dropout(0.5),
    layers.Dense(10, activation='softmax')
])
```

---

## 6. Compile the Model

```python
model.compile(
    optimizer='adam',
    loss='sparse_categorical_crossentropy',
    metrics=['accuracy']
)
```

---

## 7. Train the Model

```python
history = model.fit(
    x_train, y_train,
    epochs=10,
    validation_data=(x_test, y_test)
)
```

---

## 8. Evaluate the Model

```python
test_loss, test_acc = model.evaluate(x_test, y_test, verbose=2)
print(f"Test accuracy: {test_acc:.2f}")
```

You should get an accuracy around **70–80%** for this simple model.

---

## 9. Plot Training Results

```python
plt.plot(history.history['accuracy'], label='Training Accuracy')
plt.plot(history.history['val_accuracy'], label='Validation Accuracy')
plt.xlabel('Epoch')
plt.ylabel('Accuracy')
plt.legend()
plt.show()
```

---

## 10. Make Predictions

```python
import numpy as np

predictions = model.predict(x_test)

# Show prediction for one image
i = 0
plt.imshow(x_test[i])
plt.xlabel(f"Predicted: {class_names[np.argmax(predictions[i])]}, Actual: {class_names[int(y_test[i])]}")
plt.show()
```

---

## 11. Save and Load Your Model

```python
model.save("cnn_cifar10_model.h5")
# To load later:
# model = tf.keras.models.load_model("cnn_cifar10_model.h5")
```

---

## 12. Going Further

Try improving the model with:

* 🧩 **Data augmentation** (`tf.keras.preprocessing.image.ImageDataGenerator`)
* 🏋️ **Batch normalization** layers
* ⚙️ **Deeper CNNs** (e.g., VGG, ResNet architectures)
* 📱 **TensorFlow Lite** deployment

---
