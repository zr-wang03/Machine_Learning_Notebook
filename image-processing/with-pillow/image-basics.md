# Image Basics

### Objectives

Image processing and computer vision tasks include displaying, cropping, flipping, rotating, image segmentation, classification, image restoration, image recognition, image generation. Also, working with images via the cloud requires storing, transmitting, and gathering images through the internet.

Python is an excellent choice as it has many image processing tools, computer vision and artificial intelligence libraries. Finally, it has many libraries for working with files in the cloud and on the internet.

A digital image is simply a file in your computer. In this lab, you will gain an understanding of these files and learn to work with these files with the Pillow Library (PIL).

* Python Image Libraries
  * Image Files and Paths
  * Load in Image in Python
  * Plotting an Image
  * Gray Scale Images, Quantization and Color Channels
  * PIL Images into NumPy Arrays

***

Download the images for the lab:

```python
!wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/lenna.png -O lenna.png
!wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/baboon.png -O baboon.png
!wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/barbara.png -O barbara.png  
```

```
--2023-01-08 19:06:20--  https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/lenna.png
Resolving cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)... 198.23.119.245
Connecting to cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)|198.23.119.245|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 473831 (463K) [image/png]
Saving to: ‘lenna.png’

lenna.png           100%[===================>] 462.73K  1.00MB/s    in 0.4s    

2023-01-08 19:06:21 (1.00 MB/s) - ‘lenna.png’ saved [473831/473831]

--2023-01-08 19:06:23--  https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/baboon.png
Resolving cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)... 169.63.118.104
Connecting to cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)|169.63.118.104|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 637192 (622K) [image/png]
Saving to: ‘baboon.png’

baboon.png          100%[===================>] 622.26K  --.-KB/s    in 0.02s   

2023-01-08 19:06:23 (28.1 MB/s) - ‘baboon.png’ saved [637192/637192]

--2023-01-08 19:06:24--  https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/barbara.png
Resolving cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)... 169.63.118.104
Connecting to cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)|169.63.118.104|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 185727 (181K) [image/png]
Saving to: ‘barbara.png’

barbara.png         100%[===================>] 181.37K  --.-KB/s    in 0.01s   

2023-01-08 19:06:25 (16.2 MB/s) - ‘barbara.png’ saved [185727/185727]
```

First, let's define a helper function to concatenate two images side-by-side. You will not need to understand the code below at this moment, but this function will be used repeatedly in this tutorial to showcase the results.

```python
def get_concat_h(im1, im2):
    #https://note.nkmk.me/en/python-pillow-concat-images/
    dst = Image.new('RGB', (im1.width + im2.width, im1.height))
    dst.paste(im1, (0, 0))
    dst.paste(im2, (im1.width, 0))
    return dst
```

### Image Files and Paths

An image is stored as a file on your computer. Below, we define `my_image` as the filename of a file in this directory.

```python
my_image = "lenna.png"
```

A filename consists of two parts: the name of the file and the extension, separated by a full stop (`.`). The extension specifies the format of the Image. There are two popular image formats: Joint Photographic Expert Group image (or `.jpg`, `.jpeg`) and Portable Network Graphics (or `.png`). These file types make it simpler to work with images. For example, it compresses the image, taking less spaces on your drive to store the image.

Image files are stored in the file system of your computer. The location of it is specified using a "path", which is often unique. You can find the path of your current working directory with Python's `os` module. The `os` module provides functions to interact with the file system, e.g. creating or removing a directory (folder), listing its contents, changing and identifying the current working directory.

```python
import os
cwd = os.getcwd()
cwd 
```

```
'/resources/labs/CV0101EN'
```

The "path" to an image can be found using the following line of code.

```python
image_path = os.path.join(cwd, my_image)
image_path
```

```
'/resources/labs/CV0101EN/lenna.png'
```

### Load Images in Python

Pillow (PIL) library is a popular library for loading images in Python. In addition, many other libraries such as "Keras" and "PyTorch" use this library to work with images. The `Image` module provides functions to load images from and saving images to the file system. Let's import it from `PIL`.

```python
from PIL import Image
```

If the image is in the current working directory, you can load the image as follows using the image's filename and create a PIL Image object:

```python
image = Image.open(my_image)
type(image)
```

```
PIL.PngImagePlugin.PngImageFile
```

If you are working in a Jupyter environment, you can view the image by calling the variable itself.

```python
image
```

![png](<../../.gitbook/assets/output\_25\_0 (1)>)

### Plotting an Image

We can also use the method `show` of PIL objects to display the image. Please note this method may or may not work depending on your setup.

```python
image.show()
```

You can also use `imshow` method from the `matplotlib` library to display the image.

```python
import matplotlib.pyplot as plt
```

```python
plt.figure(figsize=(10,10))
plt.imshow(image)
plt.show()
```

![png](../../.gitbook/assets/output\_31\_0)

You can also load the image using its full path. This comes in handy if the image is not in your working directory.

```python
image = Image.open(image_path)
```

We can use the attributes of the image object to get information. The attribute format is the extension or format of the image.

The attribute `size` returns a tuple, the first element is the number of pixels that comprise the width and the second element is the number of pixels that make up the height of the image.

```python
print(image.size)
```

```
(512, 512)
```

This is a string specifying the pixel format used. In this case, it's “RGB”. RGB is a color space where red, green, and blue are added together to produce other colors.

```python
print(image.mode)
```

```
RGB
```

The `Image.open` method does not load image data into the computer memory. The `load` method of `PIL` object reads the file content, decodes it, and expands the image into memory.

```python
im = image.load() 
```

We can then check the intensity of the image at the $x$-th column and $y$-th row:

```python
x = 0
y = 1
im[y,x]
```

```
(226, 137, 125)
```

We will use the array form to access the elements; it is slightly different.

You can save the image in `jpg` format using the following command.

```python
image.save("lenna.jpg")
```

### Grayscale Images, Quantization and Color Channels

#### Grayscale Images

The `ImageOps` module contains several ‘ready-made’ image processing operations. This module is somewhat experimental, and most operators only work with grayscale and/or RGB images.

```python
from PIL import ImageOps 
```

Grayscale images have pixel values representing the amount of light or intensity of that pixel. Light shades of gray have a high-intensity while darker shades have a lower intensity, i.e, white has the highest intensity and black the lowest.

```python
image_gray = ImageOps.grayscale(image) 
image_gray 
```

![png](<../../.gitbook/assets/output\_51\_0 (2)>)

The mode is `L` for grayscale.

```python
image_gray.mode
```

```
'L'
```

#### Quantization

The Quantization of an image is the number of unique intensity values any given pixel of the image can take. For a grayscale image, this means the number of different shades of gray. Most images have 256 different levels. You can decrease the levels using the method `quantize`. Let's repeatably cut the number of levels in half and observe what happens:

Half the levels do not make a noticable difference.

```python
image_gray.quantize(256 // 2)
image_gray.show()
```

Let’s continue dividing the number of values by two and compare it to the original image.

```python
#get_concat_h(image_gray,  image_gray.quantize(256//2)).show(title="Lena") 
for n in range(3,8):
    plt.figure(figsize=(10,10))

    plt.imshow(get_concat_h(image_gray,  image_gray.quantize(256//2**n))) 
    plt.title("256 Quantization Levels  left vs {}  Quantization Levels right".format(256//2**n))
    plt.show()
```

![png](../../.gitbook/assets/output\_59\_0)

![png](../../.gitbook/assets/output\_59\_1)

![png](../../.gitbook/assets/output\_59\_2)

![png](../../.gitbook/assets/output\_59\_3)

![png](../../.gitbook/assets/output\_59\_4)

#### Color Channels

We can also work with the different color channels. Consider the following image:

```python
baboon = Image.open('baboon.png')
baboon
```

![png](../../.gitbook/assets/output\_62\_0)

We can obtain the different RGB color channels and assign them to the variables `red`, `green`, and `blue`:

```python
red, green, blue = baboon.split()
```

Plotting the color image next to the red channel as a grayscale, we see that regions with red have higher intensity values.

```python
get_concat_h(baboon, red)
```

![png](../../.gitbook/assets/output\_66\_0)

We can do the same for the blue and green channels:

```python
get_concat_h(baboon, blue)
```

![png](<../../.gitbook/assets/output\_68\_0 (1)>)

```python
get_concat_h(baboon, green)
```

![png](../../.gitbook/assets/output\_69\_0)

### PIL Images into NumPy Arrays

NumPy is a library for Python, allowing you to work with multi-dimensional arrays and matrices. We can convert a PIL image to a NumPy array. We use `asarray()` or `array` function from NumPy to convert PIL images into NumPy arrays.

First, let's import the numpy module:

```python
import numpy as np
```

We apply it to the `PIL` image we get a numpy array:

```python
array= np.asarray(image)
print(type(array))
```

```
<class 'numpy.ndarray'>
```

`np.asarray` turns the original image into a numpy array. Often, we don't want to manipulate the image directly, but instead, create a copy of the image to manipulate. The `np.array` method creates a new copy of the image, such that the original one will remain unmodified.

```python
array = np.array(image)
```

The attribute `shape` of a `numpy.array` object returns a tuple corresponding to the dimensions of it, the first element gives the number of rows or height of the image, the second is element is the number of columns or width of the image. The final element is the number of colour channels.

```python
# summarize shape
print(array.shape)
```

```
(512, 512, 3)
```

or `(rows, columns, colors)`. Each element in the color axis corresponds to the following value `(R, G, B)` format.

We can view the intensity values by printing out the array, they range from 0 to 255 or $2^{8}$ (8-bit).

```python
print(array)
```

```
[[[226 137 125]
  [226 137 125]
  [223 137 133]
  ...
  [230 148 122]
  [221 130 110]
  [200  99  90]]

 [[226 137 125]
  [226 137 125]
  [223 137 133]
  ...
  [230 148 122]
  [221 130 110]
  [200  99  90]]

 [[226 137 125]
  [226 137 125]
  [223 137 133]
  ...
  [230 148 122]
  [221 130 110]
  [200  99  90]]

 ...

 [[ 84  18  60]
  [ 84  18  60]
  [ 92  27  58]
  ...
  [173  73  84]
  [172  68  76]
  [177  62  79]]

 [[ 82  22  57]
  [ 82  22  57]
  [ 96  32  62]
  ...
  [179  70  79]
  [181  71  81]
  [185  74  81]]

 [[ 82  22  57]
  [ 82  22  57]
  [ 96  32  62]
  ...
  [179  70  79]
  [181  71  81]
  [185  74  81]]]
```

The Intensity values are 8-bit unsigned datatype.

```python
array[0, 0]
```

```
array([226, 137, 125], dtype=uint8)
```

We can find the maximum and minimum intensity value of the array:

```python
array.min()
```

```
3
```

```python
array.max()
```

```
255
```

#### Indexing

You can plot the array as an image:

```python
plt.figure(figsize=(10,10))
plt.imshow(array)
plt.show()
```

![png](../../.gitbook/assets/output\_89\_0)

We can use numpy slicing, for example, we can return the first 256 rows corresponding to the top half of the image:

```python
rows = 256
```

```python
plt.figure(figsize=(10,10))
plt.imshow(array[0:rows,:,:])
plt.show()
```

![png](../../.gitbook/assets/output\_92\_0)

We can also return the first 256 columns corresponding to the first half of the image.

```python
columns = 256
```

```python
plt.figure(figsize=(10,10))
plt.imshow(array[:,0:columns,:])
plt.show()
```

![png](../../.gitbook/assets/output\_95\_0)

If you want to reassign an array to another variable, you should use the `copy` method (we will cover this in the next section).

```python
A = array.copy()
plt.imshow(A)
plt.show()
```

![png](../../.gitbook/assets/output\_97\_0)

If we do not apply the method copy(), the variable will point to the same location in memory. Consider the array B. If we set all values of array A to zero, as B points to A, the values of B will be zero too:

```python
B = A
A[:,:,:] = 0
plt.imshow(B)
plt.show()
```

![png](../../.gitbook/assets/output\_99\_0)

We can also work with the different color channels. Consider the baboon image:

```python
baboon_array = np.array(baboon)
plt.figure(figsize=(10,10))
plt.imshow(baboon_array)
plt.show()
```

![png](../../.gitbook/assets/output\_101\_0)

We can plot the red channel as intensity values of the red channel.

```python
baboon_array = np.array(baboon)
plt.figure(figsize=(10,10))
plt.imshow(baboon_array[:,:,0], cmap='gray')
plt.show()
```

![png](../../.gitbook/assets/output\_103\_0)

Or we can create a new array and set all but the red color channels to zero. Therefore, when we display the image it appears red:

```python
baboon_red=baboon_array.copy()
baboon_red[:,:,1] = 0
baboon_red[:,:,2] = 0
plt.figure(figsize=(10,10))
plt.imshow(baboon_red)
plt.show()
```

![png](../../.gitbook/assets/output\_105\_0)

We can do the same for blue:

```python
baboon_blue=baboon_array.copy()
baboon_blue[:,:,0] = 0
baboon_blue[:,:,1] = 0
plt.figure(figsize=(10,10))
plt.imshow(baboon_blue)
plt.show()
```

![png](../../.gitbook/assets/output\_107\_0)

#### Question 1:

Use the image `lenna.png` from this lab or take any image you like.

Open the image and create a PIL Image object called `blue_lenna`, convert the image into a numpy array we can manipulate called `blue_array`, get the blue channel out of it, and finally plot the image

```python
# write your code here
blue_lenna=Image.open("lenna.png")
blue_array=np.array(blue_lenna)
blue_array[:,:,0]=0
blue_array[:,;,1]=0
plt.figure(figsize=(10,10))
plt.imshow(blue_array)
plt.show()
```

```
  File "/tmp/ipykernel_83/2413973645.py", line 5
    blue_array[:,;,1]=0
                 ^
SyntaxError: invalid syntax
```

###
