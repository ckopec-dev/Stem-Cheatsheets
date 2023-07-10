
# Deep Learning Cheatsheet

## Neural networks

- A neural network consists of an input layer on the left, one or more hidden layers in the middle, and an output layer on the right.
- Each layer contains one or more neurons (a.k.a. nodes).
- Neurons are connected to one or more other neurons and each connection has a weight associated with it.

## Feed forward propagation

- The forward propagation calculation proceeds from left to right, layer-wise.
- To compute neuron values: for each connected neuron in the preceeding layer, multiply the neuron's value by the weight of the target neuron. Add each result to the target neuron value.

## Activation functions

- An activation function determines if the neuron fires.
- A popular modern one is called Rectified Linear Activation (ReLU): returns 0 if x < 0, otherwise returns x.

## Loss functions

- Aggregates errors in predictions into a single number
- It's a measure of a model's predictive performance
- Lower = better

### Gradient descent

- Look around for nearby downhill slopes (go opposite the slope, if the slope is positive)
- Repeat until uphill in every direction

### Learning rate

- Update every weight by subtracting learning rate * slope

## Backpropagation

- Sends error from output layer back through previous layers and updates the weights accordlingly
