
# Scikit-image Cheatsheets

## Installation

`$ pip3 install scimage-image`

## Basics

~~~

# Imports
from skimage import data, color
from matplotlib import pyplot as plt

# Load a built-in image
rocket_image = data.rocket()

# Convert color to grayscale
grayscale = color.rgb2gray(original)

# Convert grayscale to color
rbg = color.gray2rgb(grayscale)

# Display an image
plt.imshow(image)
plt.show()

~~~


