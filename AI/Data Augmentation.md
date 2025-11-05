
# 🧠 TensorFlow Tutorial: Data Augmentation for Image Classification

---

## 1. What Is Data Augmentation?

**Data augmentation** is a technique to artificially expand your training dataset by creating modified copies of existing images.

✅ **Why it matters:**

* Increases dataset diversity
* Reduces overfitting
* Improves generalization to unseen data

Common transformations include:

* Flipping (horizontal/vertical)
* Rotating
* Zooming
* Shifting (translation)
* Changing brightness or contrast
* Adding noise

---

## 2. Install and Import Dependencies

```bash
pip install tensorflow matplotlib
```

Then in Python:

```python
import tensorflow as tf
from tensorflow.keras import datasets, layers, models
import matplotlib.pyplot as plt
```

---

## 3. Load a Dataset (CIFAR-10 Example)

We’ll use the **CIFAR-10** dataset again — small color images in 10 classes.

```python
(x_train, y_train), (x_test, y_test) = datasets.cifar10.load_data()
x_train, x_test = x_train / 255.0, x_test / 255.0
```

---

## 4. Create a Data Augmentation Pipeline

Keras provides a convenient layer-based API for augmentation.

```python
data_augmentation = tf.keras.Sequential([
    layers.RandomFlip("horizontal"),      # Flip horizontally
    layers.RandomRotation(0.1),           # Rotate ±10%
    layers.RandomZoom(0.1),               # Zoom in/out
    layers.RandomContrast(0.1),           # Adjust contrast
])
```

---

## 5. Visualize Augmented Images

Let’s see what augmentation looks like.

```python
plt.figure(figsize=(10,10))
for i in range(9):
    augmented_image = data_augmentation(x_train[i:i+1])  # Single image batch
    plt.subplot(3,3,i+1)
    plt.imshow(augmented_image[0])
    plt.axis('off')
plt.show()
```

This shows 9 different random transformations of your images — generated **on the fly**.

---

## 6. Build a CNN Model with Augmentation

You can add the augmentation layer directly into your model:

```python
model = models.Sequential([
    data_augmentation,                    # <— Augmentation happens here
    layers.Conv2D(32, (3,3), activation='relu', input_shape=(32,32,3)),
    layers.MaxPooling2D((2,2)),
    layers.Conv2D(64, (3,3), activation='relu'),
    layers.MaxPooling2D((2,2)),
    layers.Conv2D(64, (3,3), activation='relu'),
    layers.Flatten(),
    layers.Dense(64, activation='relu'),
    layers.Dropout(0.5),
    layers.Dense(10, activation='softmax')
])
```

---

## 7. Compile the Model

```python
model.compile(
    optimizer='adam',
    loss='sparse_categorical_crossentropy',
    metrics=['accuracy']
)
```

---

## 8. Train the Model

```python
history = model.fit(
    x_train, y_train,
    epochs=15,
    validation_data=(x_test, y_test)
)
```

You’ll notice:

* Training accuracy might be slightly lower (due to more variability)
* Validation accuracy usually improves — **less overfitting!**

---

## 9. Compare With and Without Augmentation

To see the effect, train another model **without** the `data_augmentation` layer and compare:

```python
model_no_aug = models.Sequential([
    layers.Conv2D(32, (3,3), activation='relu', input_shape=(32,32,3)),
    layers.MaxPooling2D((2,2)),
    layers.Conv2D(64, (3,3), activation='relu'),
    layers.MaxPooling2D((2,2)),
    layers.Flatten(),
    layers.Dense(64, activation='relu'),
    layers.Dense(10, activation='softmax')
])

model_no_aug.compile(optimizer='adam',
                     loss='sparse_categorical_crossentropy',
                     metrics=['accuracy'])

history_no_aug = model_no_aug.fit(
    x_train, y_train,
    epochs=15,
    validation_data=(x_test, y_test)
)
```

Then plot comparison:

```python
plt.plot(history.history['val_accuracy'], label='With Augmentation')
plt.plot(history_no_aug.history['val_accuracy'], label='Without Augmentation')
plt.xlabel('Epoch')
plt.ylabel('Validation Accuracy')
plt.legend()
plt.show()
```

You should see that the augmented model **generalizes better**.

---

## 10. More Advanced Augmentation Options

* **Custom transformations**:

  ```python
  layers.RandomBrightness(factor=0.2)
  layers.RandomTranslation(height_factor=0.1, width_factor=0.1)
  ```

* **ImageDataGenerator (older API)**:

  ```python
  from tensorflow.keras.preprocessing.image import ImageDataGenerator
  datagen = ImageDataGenerator(rotation_range=20, horizontal_flip=True)
  datagen.fit(x_train)
  ```
  
* **Augmenting only during training** ensures test data stays clean.

---

## 11. Save and Use the Augmented Model

```python
model.save("cnn_with_augmentation.h5")
# model = tf.keras.models.load_model("cnn_with_augmentation.h5")
```

---

## 12. Key Takeaways

✅ Data augmentation is a **low-cost way** to improve model performance.
✅ It prevents **overfitting** and increases **robustness**.
✅ TensorFlow’s `layers.Random*` API makes it **simple and fast**.

