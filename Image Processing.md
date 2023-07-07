
# Image Processing Cheatsheets

## Installation of skimage-image

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

## Using numpy with images

### Load an image into an ndarray

`image = plt.imread('{path}')`

### Obtain a single channel of an ndarray image

`red = image[:, :, 0]`

### Show the shape 

`image.shape`

### Show the number of pixels

`image.size`

### Flip vertically

`flipped = np.flipup(image)`

### Flip horizontally

`flipped = np.fliplr(image)`

