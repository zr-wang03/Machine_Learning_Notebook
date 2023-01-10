---
description: From IBM Skills Network
---

# Image Basics

## OpenCV Library

### Objectives

Image processing and computer vision tasks include displaying, cropping, flipping, rotating, image segmentation, classification, image restoration, image recognition, image generation. Also, working with images via the cloud requires storing and transmitting, and gathering images through the internet. Python is an excellent choice as it has many image processing tools, computer vision, and artificial intelligence libraries. Finally, it has many libraries for working with files in the cloud and the internet. A digital image is simply a file on your computer. In this lab, you will gain an understanding of these files and learn to work with these files with some popular libraries

* Open CV
  * Image Files and Paths
  * Load in Image in Python
  * Plotting an Image
  * Gray Scale Images, Quantization and Color Channels
  * Gray Scale Images, Quantization and Color Channels

***

Download the image for the lab:

```python
!wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/lenna.png -O lenna.png
!wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/baboon.png -O baboon.png
!wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/barbara.png -O barbara.png  
```

```
--2023-01-08 20:10:45--  https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/lenna.png
Resolving cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)... 169.63.118.104
Connecting to cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)|169.63.118.104|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 473831 (463K) [image/png]
Saving to: ‘lenna.png’

lenna.png           100%[===================>] 462.73K  --.-KB/s    in 0.02s   

2023-01-08 20:10:45 (27.7 MB/s) - ‘lenna.png’ saved [473831/473831]

--2023-01-08 20:10:47--  https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/baboon.png
Resolving cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)... 169.63.118.104
Connecting to cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)|169.63.118.104|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 637192 (622K) [image/png]
Saving to: ‘baboon.png’

baboon.png          100%[===================>] 622.26K  --.-KB/s    in 0.03s   

2023-01-08 20:10:47 (23.8 MB/s) - ‘baboon.png’ saved [637192/637192]

--2023-01-08 20:10:49--  https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/barbara.png
Resolving cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)... 169.63.118.104
Connecting to cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)|169.63.118.104|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 185727 (181K) [image/png]
Saving to: ‘barbara.png’

barbara.png         100%[===================>] 181.37K  --.-KB/s    in 0.008s  

2023-01-08 20:10:49 (22.2 MB/s) - ‘barbara.png’ saved [185727/185727]
```

First, let's define a helper function to concatenate two images side-by-side. You will need to understand this code this moment, but this function will be used repeatedly in this tutorial to showcase the results.

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

Filename consists of two parts, the name of the file and the extension, separated by a full stop (`.`). The extension specifies the format of the image. There are two popular image formats -- Joint Photographic Expert Group image (or `.jpg`, `.jpeg`) and Portable Network Graphics (or `.png`). These file types make it simpler to work with images. For example, it compresses the image using sine/cosine approximations, taking less spaces on your drive to store the image.

Image files are stored in the file system of your computer. The location of it is specified using a "path", which is often unique. You can find the path of your current working directory with Python's `os` module. The `os` module provides functions to interact with the file system, e.g. creating or removing a directory (folder), listing its contents, changing and identifying the current working directory.

```python
import os
cwd = os.getcwd()
```

The "path" to an image can be found using the following line of code.

```python
image_path = os.path.join(cwd, my_image)
```

### Load in Image in Python

OpenCV is a library used for computer vision. It has more functionality than the `PIL` library but is more difficult to use. We can import `OpenCV` as follows:

```python
import cv2
```

The `imread()` method loads an image from the specified file, the input is the `path` of the image to be read (just like PIL), the `flag` paramter specifies how the image should be read, and the default value is `cv2.IMREAD_COLOR`.

```python
image = cv2.imread(my_image)
```

The result is a numpy array with intensity values as 8-bit unsigned integers.

```python
type(image)
```

```
numpy.ndarray
```

We can get the shape of the array from the `shape` attribute.

```python
image.shape
```

```
(512, 512, 3)
```

The shape is the same as the PIL array, but there are several differences; for example, PIL returns in (R, G, B) format whereas OpenCV returns in (B, G, R) format.

Each pixel could take on 256 possible values as intensity, ranging from 0 to 255, with 0 being the lowest intensity and 255 being the highest.&#x20;

### Plotting an Image

You can use the `imshow` function from the `matplotlib` library:

```python
import matplotlib.pyplot as plt
```

```python
plt.figure(figsize=(10,10))
plt.imshow(image)
plt.show()
```

![png](<../../.gitbook/assets/output\_38\_0 (1)>)

The image output doesn't look natural. This is because the order of RGB Channels are different. We can change the color space with conversion code and the function `cvtColor` from the `cv2` library:

```python
new_image=cv2.cvtColor(image, cv2.COLOR_BGR2RGB)
```

```python
plt.figure(figsize=(10,10))
plt.imshow(new_image)
plt.show()
```

![png](<../../.gitbook/assets/output\_41\_0 (1)>)

You can also load the image using its path, this comes in handy if the image is not in your working directory:

```python
image = cv2.imread(image_path)
image.shape
```

```
(512, 512, 3)
```

You can save the image as in `jpg` format.

```python
cv2.imwrite("lenna.jpg", image)
```

#### Grayscale Images

Grayscale images have pixel values representing the amount of light or intensity. Light shades of gray have a high-intensity darker shades have a lower intensity. White has the highest intensity, and black the lowest. We can convert an image to Gray Scale using a color conversion code and the function `cvtColor`.

The code for RGB to gray is `cv2.COLOR_BGR2GRAY`, we apply the function:

```python
image_gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
```

The image array has only two dimensions, i.e. only one color channel:

```python
image_gray.shape
```

```
(512, 512)
```

We can plot the image using `imshow` but we have to specify the color map is gray:

```python
plt.figure(figsize=(10, 10))
plt.imshow(image_gray, cmap='gray')
plt.show()
```

![png](<../../.gitbook/assets/output\_53\_0 (1)>)

We can save the image as a grayscale image, let's save it as a `jpg` as well, in the working directory.

```python
cv2.imwrite('lena_gray_cv.jpg', image_gray)
```

```
True
```

You can also load in a grayscale image we have to set `flag` parameter to gray color conversation code: `cv2.COLOR_BGR2GRAY`:

```python
im_gray = cv2.imread('barbara.png', cv2.IMREAD_GRAYSCALE)
```

We can plot the image:

```python
plt.figure(figsize=(10,10))
plt.imshow(im_gray,cmap='gray')
plt.show()
```

![png](<../../.gitbook/assets/output\_59\_0 (1)>)

#### Color Channels

We can also work with the different color channels. Consider the following image:

```python
baboon=cv2.imread('baboon.png')
plt.figure(figsize=(10,10))
plt.imshow(cv2.cvtColor(baboon, cv2.COLOR_BGR2RGB))
plt.show()
```

![png](<../../.gitbook/assets/output\_62\_0 (2)>)

We can obtain the different RGB colors and assign them to the variables `blue`, `green`, and `red`, in (B, G, R) format.

```python
blue, green, red = baboon[:, :, 0], baboon[:, :, 1], baboon[:, :, 2]
```

We can concatenate each image channel the images using the function `vconcat`.

```python
im_bgr = cv2.vconcat([blue, green, red])
```

Plotting the color image next to the red channel in grayscale, we see that regions with red have higher intensity values.

```python
plt.figure(figsize=(10,10))
plt.subplot(121)
plt.imshow(cv2.cvtColor(baboon, cv2.COLOR_BGR2RGB))
plt.title("RGB image")
plt.subplot(122)
plt.imshow(im_bgr,cmap='gray')
plt.title("Different color channels  blue (top), green (middle), red (bottom)  ")
plt.show()
```

![png](../../.gitbook/assets/output\_68\_0)

#### Indexing

We can use numpy slicing. For example, we can return the first 256 rows corresponding to the top half of the image:

```python
rows = 256
```

```python
plt.figure(figsize=(10,10))
plt.imshow(new_image[0:rows,:,:])
plt.show()
```

![png](../../.gitbook/assets/output\_72\_0)

We can also return the first 256 columns corresponding to the first half of the image:

```python
columns = 256
```

```python
plt.figure(figsize=(10,10))
plt.imshow(new_image[:,0:columns,:])
plt.show()
```

![png](../../.gitbook/assets/output\_75\_0)

If you want to reassign an array to another variable, you should use the `copy` method (we will cover this in the next section).

```python
A = new_image.copy()
plt.imshow(A)
plt.show()
```

![png](../../.gitbook/assets/output\_77\_0)

If we do not apply the method `copy()`, the variable will point to the same location in memory. Consider the variable `B` below, if we set all values of array `A` to zero, since `A` and `B` points to the same object in the memory, `B` will also have all-zero elements:

```python
B = A
A[:,:,:] = 0
plt.imshow(B)
plt.show()
```

![png](../../.gitbook/assets/output\_79\_0)

We can also manipulate elements using indexing. In the following piece of code, we create a new array `baboon_red` and set all but the red color channels to zero. Therefore, when we display the image, it appears red:

```python
baboon_red = baboon.copy()
baboon_red[:, :, 0] = 0
baboon_red[:, :, 1] = 0
plt.figure(figsize=(10, 10))
plt.imshow(cv2.cvtColor(baboon_red, cv2.COLOR_BGR2RGB))
plt.show()
```

![png](../../.gitbook/assets/output\_81\_0)

We can do the same for blue:

```python
baboon_blue = baboon.copy()
baboon_blue[:, :, 1] = 0
baboon_blue[:, :, 2] = 0
plt.figure(figsize=(10, 10))
plt.imshow(cv2.cvtColor(baboon_blue, cv2.COLOR_BGR2RGB))
plt.show()
```

![png](../../.gitbook/assets/output\_83\_0)

We can do the same for green:

```python
baboon_green = baboon.copy()
baboon_green[:, :, 0] = 0
baboon_green[:, :, 2] = 0
plt.figure(figsize=(10,10))
plt.imshow(cv2.cvtColor(baboon_green, cv2.COLOR_BGR2RGB))
plt.show()
```

![png](../../.gitbook/assets/output\_85\_0)

```python
image=cv2.imread('baboon.png')
```

```python
plt.figure(figsize=(10,10))
plt.imshow(cv2.cvtColor(image, cv2.COLOR_BGR2RGB))
plt.show()
```

![png](../../.gitbook/assets/output\_87\_0)

```python
image=cv2.imread('baboon.png') # replace and add you image here name 
baboon_blue=image.copy()
baboon_blue[:,:,1] = 0
baboon_blue[:,:,2] = 0
plt.figure(figsize=(10,10))
plt.imshow(cv2.cvtColor(baboon_blue, cv2.COLOR_BGR2RGB))
plt.show()
```

![png](../../.gitbook/assets/output\_88\_0)

#### Question 1:

Use the image `baboon.png` from this lab or take any image you like.

Open the image and create a OpenCV Image object called `baboon_blue`, convert the image from BGR format to RGB format, get the blue channel out of it, and finally plot the image

```python
# write your script here
image=cv2.imread('baboon.png')

image=cv2.cvtColor(image, cv2.COLOR_BGR2RGB)
baboon_blue=image.copy()
baboon_blue[:,:,0]=0
baboon_blue[:,:,1]=0
plt.figure(figsize=(10,10))
plt.imshow(baboon_blue)
plt.show()


```

![png](../../.gitbook/assets/output\_90\_0)



###
