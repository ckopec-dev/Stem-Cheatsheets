
# Deep Learning Cheatsheets

## Neural networks

- A neural network consists of an input layer on the left, one or more hidden layers in the middle, and an output layer on the right.
- Each layer contains one or more neurons (a.k.a. nodes).
- Neurons are connected to one or more other neurons and each connection has a weight associated with it.

## Forward propagation

- The forward propagation calculation proceeds from left to right, layer-wise.
- To compute neuron values: for each connected neuron in the preceeding layer, multiply the neuron's value by the weight of the target neuron. Add each result to the target neuron value.

### Sample code

~~~

import numpy as np

input_data = np.array([2, 3])
weights = {'node_0': np.array([1, 1]), 'node_1': np.array([-1, 1]), 'output': np.array([2, -1])}

node_0_value = (input_data * weights['node_0']).sum()
node_1_value = (input_data * weights['node_1']).sum()

~~~
