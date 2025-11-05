# Tensorflow Cheatsheet

## Installation

### Run in a docker container with Jupyter

`$ docker run -it --rm -v $(realpath ~/notebooks):/tf/notebooks -p 8888:8888 tensorflow/tensorflow:latest-jupyter`

### Run in a detached docker container with Jupyter

~~~bash
$ docker run -d --rm -v $(realpath ~/notebooks):/tf/notebooks -p 8888:8888 tensorflow/tensorflow:latest-jupyter
# Browse to the IP address of the host, e.g. 192.168.1.100:8888.
$ docker logs <container-id>
# Authenticate in the browser with the token specified in the logs.
~~~

---

## 1. What is TensorFlow?

**TensorFlow** is an open-source machine learning library developed by Google.
It’s used for:

* Deep learning (neural networks)
* Computer vision
* Natural language processing (NLP)
* Reinforcement learning
* And much more

TensorFlow can run on CPUs, GPUs, and even TPUs (specialized AI hardware).

---

## 2. Install TensorFlow

Run this in your terminal or notebook:

`pip install tensorflow`

To check your installation:

~~~python
import tensorflow as tf
print(tf.__version__)
~~~

✅ You should see something like `2.18.0` or later.

---

## 3. Load a Dataset (MNIST Example)

TensorFlow comes with popular datasets built in.
Let’s load the **MNIST** dataset — images of handwritten digits (0–9).

~~~python
import tensorflow as tf
from tensorflow.keras import datasets

# Load dataset
(x_train, y_train), (x_test, y_test) = datasets.mnist.load_data()

# Normalize pixel values (0–255 → 0–1)
x_train, x_test = x_train / 255.0, x_test / 255.0
~~~

---

## 4. Build a Simple Neural Network

We’ll use the high-level **Keras API**, which is built into TensorFlow.

~~~python
from tensorflow.keras import models, layers

# Define the model
model = models.Sequential([
    layers.Flatten(input_shape=(28, 28)),  # Flatten 28x28 images to 1D
    layers.Dense(128, activation='relu'),  # Hidden layer
    layers.Dropout(0.2),                   # Prevent overfitting
    layers.Dense(10, activation='softmax') # Output layer (10 classes)
])
~~~

---

## 5. Compile the Model

Define how the model learns (optimizer, loss, metrics):

~~~python
model.compile(
    optimizer='adam',
    loss='sparse_categorical_crossentropy',
    metrics=['accuracy']
)
~~~

---

## 6. Train the Model

`model.fit(x_train, y_train, epochs=5)`

This will train for 5 passes (epochs) over the dataset.

---

## 7. Evaluate the Model

Test on unseen data:

`model.evaluate(x_test, y_test)`

You’ll see something like:

`313/313 [=====] - 0s 89% - loss: 0.08 - accuracy: 0.98`

✅ That means ~98% accuracy!

---

## 8. Make Predictions

~~~python
predictions = model.predict(x_test)
print(tf.argmax(predictions[0]).numpy())  # Predicted digit for first test image
~~~

---

## 9. Save and Load the Model

Save:

`model.save("mnist_model.h5")`

Load later:

`model = tf.keras.models.load_model("mnist_model.h5")`

---

## 10. Next Steps

Once you understand this basic workflow, you can explore:

* 🧩 **Convolutional Neural Networks (CNNs)** for image recognition
* 💬 **Recurrent Neural Networks (RNNs)** for text and sequences
* 🧮 **Custom training loops** using `tf.GradientTape`
* 🚀 **TensorFlow Lite** for deploying models to mobile or IoT devices
