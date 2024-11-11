
# Image Processing Cheatsheet

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

### Show histogram for the red channel

~~~

red = image[:, :, 0]
plt.hist(red.ravel(), bins=256)

~~~

### Edge detection with sobel

~~~

from skimage.filters import sobel
edge_sobel = sobel(img)

~~~

### Edge detection with canny

~~~

from skimage.feature import canny

# Need to convert to grayscale first.
# sigma varies from 0 to 1. Lower sigma means more edges.
canny_edges = canny(grayscale_image, sigma=0.5)

~~~

### Corner detection with harris

~~~

from skimage.feature import corner_harris

# Need to convert to grayscale first.
measure_image = corner_harris(grayscale_image)

# Return the corner peaks.
# min_distance is the minimum separation of the corners.
coords = corner_peaks(corner_harris(grayscale_image), min_distance=5)

~~~

### Face detection

~~~

from skimage.feature import Cascade

# Load the built-in trained detection file
trained_file = data.lbp_frontal_face_cascade_filename()

detector = Cascade(trained_file)

detected = detector.detect_multi_scale(img=image, scale_factor=1.2, step_ratio=1, min_size=(10, 10), max_size=(200, 200))

print(detected)

~~~

### Function to show detected faces

~~~ 

def show_detected_face(result, detected, title="Face image"):
    plt.imshow(result)
    img_desc = plt.gca()
    plt.set_cmap('gray')
    plt.title(title)
    plt.axis('off')

    for patch in detected:
        img_desc.add_patch(
            patches.Rectangle(
                (patch['c'], patch['r']),
                patch['width'],
                patch['height'],
                fill=False,color='r',linewidth=2)
        )
    plt.show()

~~~

## Metadata

### Extract all image metadata from a directory containing images into a csv file

~~~

# If needed:
$ sudo apt install exiftool

# Extract:
$ exiftool -csv >{MyCsv.csv} {directory_containing_images}

# E.g.
$ exiftool -csv >out.txt ~/Code/images/*

# For quick analysis, import into Access database and query table.

~~~
