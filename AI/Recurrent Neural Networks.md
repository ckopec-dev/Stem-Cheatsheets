
# 🧠 TensorFlow Tutorial: Recurrent Neural Networks (RNNs)

---

## 1. What Is an RNN?

A **Recurrent Neural Network (RNN)** is a neural network designed to process **sequential data** — where each element depends on previous ones.

Examples:

* 📝 Text (sentences, documents)
* 🎵 Music or speech signals
* 📈 Time series (stock prices, weather, sensors)

### Key Idea

Unlike regular feed-forward networks, RNNs have a *memory* — they keep track of previous inputs through a hidden state:

[
h_t = f(Wx_t + Uh_{t-1} + b)
]

---

## 2. Install and Import Dependencies

```bash
pip install tensorflow matplotlib
```

Then in Python:

```python
import tensorflow as tf
from tensorflow.keras import layers, models, datasets, preprocessing
import matplotlib.pyplot as plt
```

---

## 3. Load a Dataset (IMDB Movie Reviews)

We’ll use TensorFlow’s **IMDB sentiment analysis dataset** — binary classification of movie reviews as positive/negative.

```python
max_features = 10000   # Only keep the top 10k most common words
max_len = 200          # Only use the first 200 words per review

(x_train, y_train), (x_test, y_test) = tf.keras.datasets.imdb.load_data(num_words=max_features)

# Pad sequences to the same length
x_train = tf.keras.preprocessing.sequence.pad_sequences(x_train, maxlen=max_len)
x_test = tf.keras.preprocessing.sequence.pad_sequences(x_test, maxlen=max_len)
```

---

## 4. Build a Simple RNN Model

We’ll start with a **basic RNN layer**.

```python
model_rnn = models.Sequential([
    layers.Embedding(input_dim=max_features, output_dim=64, input_length=max_len),
    layers.SimpleRNN(64, return_sequences=False),
    layers.Dense(1, activation='sigmoid')
])
```

---

## 5. Compile the Model

```python
model_rnn.compile(
    optimizer='adam',
    loss='binary_crossentropy',
    metrics=['accuracy']
)

model_rnn.summary()
```

---

## 6. Train the RNN

```python
history_rnn = model_rnn.fit(
    x_train, y_train,
    epochs=5,
    batch_size=64,
    validation_data=(x_test, y_test)
)
```

This might take a few minutes on CPU — RNNs process one time step at a time.

---

## 7. Plot Training Progress

```python
plt.plot(history_rnn.history['accuracy'], label='Training Accuracy')
plt.plot(history_rnn.history['val_accuracy'], label='Validation Accuracy')
plt.xlabel('Epoch')
plt.ylabel('Accuracy')
plt.legend()
plt.show()
```

You should see validation accuracy around **80–85%** for this small model.

---

## 8. Using **LSTM** or **GRU** Layers (Better RNNs)

Basic RNNs often suffer from **vanishing gradients** for long sequences.
Modern variants — **LSTM (Long Short-Term Memory)** and **GRU (Gated Recurrent Unit)** — solve this.

### 🔹 LSTM Example

```python
model_lstm = models.Sequential([
    layers.Embedding(max_features, 64),
    layers.LSTM(64, dropout=0.2, recurrent_dropout=0.2),
    layers.Dense(1, activation='sigmoid')
])
```

### 🔹 GRU Example

```python
model_gru = models.Sequential([
    layers.Embedding(max_features, 64),
    layers.GRU(64, dropout=0.2, recurrent_dropout=0.2),
    layers.Dense(1, activation='sigmoid')
])
```

These models train faster and remember longer dependencies.

---

## 9. Train an LSTM Model

```python
model_lstm.compile(optimizer='adam', loss='binary_crossentropy', metrics=['accuracy'])
history_lstm = model_lstm.fit(
    x_train, y_train,
    epochs=5,
    batch_size=64,
    validation_data=(x_test, y_test)
)
```

---

## 10. Compare RNN vs LSTM Performance

```python
plt.plot(history_rnn.history['val_accuracy'], label='Simple RNN')
plt.plot(history_lstm.history['val_accuracy'], label='LSTM')
plt.xlabel('Epoch')
plt.ylabel('Validation Accuracy')
plt.legend()
plt.show()
```

✅ You’ll notice LSTM maintains higher and more stable accuracy over time.

---

## 11. Making Predictions

```python
sample = x_test[0:1]
prediction = model_lstm.predict(sample)
print("Predicted sentiment:", "Positive" if prediction[0] > 0.5 else "Negative")
```

---

## 12. When to Use Each Type

| Layer Type    | Strength                        | Use Case                   |
| ------------- | ------------------------------- | -------------------------- |
| **SimpleRNN** | Fast, simple                    | Short sequences            |
| **LSTM**      | Captures long-term dependencies | Text, speech, sequences    |
| **GRU**       | Lightweight, efficient          | Similar to LSTM but faster |

---

## 13. Optional: Stacked (Deeper) RNNs

You can make RNNs deeper by stacking multiple recurrent layers:

```python
model_stacked = models.Sequential([
    layers.Embedding(max_features, 128),
    layers.LSTM(64, return_sequences=True),
    layers.LSTM(64),
    layers.Dense(1, activation='sigmoid')
])
```

This captures both *short-term* and *long-term* patterns.

---

## 14. Save and Load the Model

```python
model_lstm.save("rnn_sentiment_model.h5")
# model = tf.keras.models.load_model("rnn_sentiment_model.h5")
```

---

## 15. Key Takeaways

✅ RNNs process **sequences** step-by-step
✅ LSTM and GRU fix **vanishing gradient** problems
✅ Great for **text, time series, speech, and sequential signals**
✅ TensorFlow/Keras make RNNs easy to build and train
