
# 🧠 TensorFlow Tutorial: Custom Training Loops

---

## 1. What Is a Custom Training Loop?

Normally, we train models with:

```python
model.fit(x_train, y_train, epochs=5)
```

That’s the **Keras “fit” API** — simple, but limited if you want to:

* Implement **custom loss functions**
* Log metrics differently
* Use **gradient clipping**, mixed precision, or multiple optimizers
* Perform **reinforcement learning**, **GAN training**, or **meta-learning**

Custom training loops (using `tf.GradientTape`) let you define every step manually.

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

## 3. Load and Prepare the Data

We’ll use the **MNIST** dataset for simplicity.

```python
(x_train, y_train), (x_test, y_test) = datasets.mnist.load_data()
x_train, x_test = x_train / 255.0, x_test / 255.0

# Add a channel dimension (for Conv2D)
x_train = x_train[..., tf.newaxis]
x_test = x_test[..., tf.newaxis]

train_ds = tf.data.Dataset.from_tensor_slices((x_train, y_train)).shuffle(10000).batch(64)
test_ds = tf.data.Dataset.from_tensor_slices((x_test, y_test)).batch(64)
```

---

## 4. Define a Simple CNN Model

```python
model = models.Sequential([
    layers.Conv2D(32, 3, activation='relu', input_shape=(28, 28, 1)),
    layers.MaxPooling2D(),
    layers.Flatten(),
    layers.Dense(64, activation='relu'),
    layers.Dense(10)
])
```

---

## 5. Define the Loss, Optimizer, and Metrics

```python
loss_fn = tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True)
optimizer = tf.keras.optimizers.Adam()

train_loss = tf.keras.metrics.Mean(name='train_loss')
train_accuracy = tf.keras.metrics.SparseCategoricalAccuracy(name='train_accuracy')

test_loss = tf.keras.metrics.Mean(name='test_loss')
test_accuracy = tf.keras.metrics.SparseCategoricalAccuracy(name='test_accuracy')
```

---

## 6. Create Custom Training and Test Steps

We’ll use **`tf.GradientTape`** to compute gradients manually.

```python
@tf.function
def train_step(images, labels):
    with tf.GradientTape() as tape:
        predictions = model(images, training=True)
        loss = loss_fn(labels, predictions)
    gradients = tape.gradient(loss, model.trainable_variables)
    optimizer.apply_gradients(zip(gradients, model.trainable_variables))

    train_loss(loss)
    train_accuracy(labels, predictions)

@tf.function
def test_step(images, labels):
    predictions = model(images, training=False)
    t_loss = loss_fn(labels, predictions)

    test_loss(t_loss)
    test_accuracy(labels, predictions)
```

---

## 7. Run the Custom Training Loop

```python
EPOCHS = 5

for epoch in range(EPOCHS):
    # Reset metrics at the start of each epoch
    train_loss.reset_state()
    train_accuracy.reset_state()
    test_loss.reset_state()
    test_accuracy.reset_state()

    # Training loop
    for images, labels in train_ds:
        train_step(images, labels)

    # Testing loop
    for test_images, test_labels in test_ds:
        test_step(test_images, test_labels)

    print(
        f"Epoch {epoch+1}, "
        f"Loss: {train_loss.result():.4f}, "
        f"Accuracy: {train_accuracy.result()*100:.2f}%, "
        f"Test Loss: {test_loss.result():.4f}, "
        f"Test Accuracy: {test_accuracy.result()*100:.2f}%"
    )
```

✅ You’ll see per-epoch results just like with `model.fit()` — but now *you* control everything.

---

## 8. Add Custom Behavior (Optional)

Want to do something special each step? Easy.

### Example: Gradient Clipping

```python
gradients = tape.gradient(loss, model.trainable_variables)
gradients = [tf.clip_by_norm(g, 1.0) for g in gradients]
optimizer.apply_gradients(zip(gradients, model.trainable_variables))
```

### Example: Custom Learning Rate Schedule

```python
lr = 1e-3 * (0.95 ** epoch)
optimizer.learning_rate.assign(lr)
```

---

## 9. Visualize Accuracy

```python
plt.plot(range(1, EPOCHS+1), [train_accuracy.result() for _ in range(EPOCHS)], label='Train Accuracy')
plt.plot(range(1, EPOCHS+1), [test_accuracy.result() for _ in range(EPOCHS)], label='Test Accuracy')
plt.xlabel('Epoch')
plt.ylabel('Accuracy')
plt.legend()
plt.show()
```

---

## 10. Save and Load the Model

```python
model.save("custom_training_model.h5")
# To load later:
# model = tf.keras.models.load_model("custom_training_model.h5")
```

---

## 11. When to Use Custom Training Loops

✅ **Use `model.fit()`** for standard supervised learning tasks.
✅ **Use custom loops** when you need:

* GAN training (two models with competing objectives)
* Reinforcement learning
* Multi-loss systems
* Research experiments where you log custom metrics or behaviors

---

## 12. Key Takeaways

| Concept                 | Description                                                  |
| ----------------------- | ------------------------------------------------------------ |
| **`tf.GradientTape`**   | Records operations to compute gradients                      |
| **Manual optimization** | Gives full control over updates                              |
| **Custom loops**        | Enable advanced ML workflows                                 |
| **Performance**         | Still uses TensorFlow’s graph optimizations (`@tf.function`) |
