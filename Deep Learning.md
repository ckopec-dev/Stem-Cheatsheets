
# Deep Learning Cheatsheets

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

## Sample code

~~~

import numpy as np

input_data = np.array([2, 3])
weights = {'node_0': np.array([1, 1]), 'node_1': np.array([-1, 1]), 'output': np.array([2, -1])}

node_0_value = (input_data * weights['node_0']).sum()
node_1_value = (input_data * weights['node_1']).sum()

hidden_layer_outputs = np.array([node_0_output, node_1_output])

model_output = (hidden_layer_outputs * weights['output']).sum()

~~~
