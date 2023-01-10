# Spatial Filtering

## Geometric Operations and Other Mathematical Tools with OpenCV

### Spatial Operations in Image Processing

Spatial operations use pixels in a neighborhood to determine the present pixel value. Applications include filtering and sharpening. They are used in many steps in computer vision like segmentation and are a key building block in Artificial Intelligence algorithms.

* Linear Filtering
  * Filtering Noise
  * Gaussian Blur
  * Image Sharpening
* Edges
* Median
* Threshold

***

Download the images for the lab

```python
!wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/cameraman.jpeg
!wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/lenna.png
!wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/barbara.png   
```

```
--2023-01-10 03:50:12--  https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/cameraman.jpeg
Resolving cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)... 169.63.118.104
Connecting to cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)|169.63.118.104|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 7243 (7.1K) [image/jpeg]
Saving to: ‘cameraman.jpeg.2’

cameraman.jpeg.2    100%[===================>]   7.07K  --.-KB/s    in 0s      

2023-01-10 03:50:12 (55.3 MB/s) - ‘cameraman.jpeg.2’ saved [7243/7243]

--2023-01-10 03:50:12--  https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/lenna.png
Resolving cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)... 169.63.118.104
Connecting to cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)|169.63.118.104|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 473831 (463K) [image/png]
Saving to: ‘lenna.png.3’

lenna.png.3         100%[===================>] 462.73K  --.-KB/s    in 0.01s   

2023-01-10 03:50:12 (39.2 MB/s) - ‘lenna.png.3’ saved [473831/473831]

--2023-01-10 03:50:13--  https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/barbara.png
Resolving cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)... 169.63.118.104
Connecting to cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)|169.63.118.104|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 185727 (181K) [image/png]
Saving to: ‘barbara.png.3’

barbara.png.3       100%[===================>] 181.37K  --.-KB/s    in 0.005s  

2023-01-10 03:50:13 (34.5 MB/s) - ‘barbara.png.3’ saved [185727/185727]
```

We will import the following libraries

```python
# Used to view the images
import matplotlib.pyplot as plt
# Used to perform filtering on an image
import cv2
# Used to create kernels for filtering
import numpy as np
```

This function will plot two images side by side

```python
def plot_image(image_1, image_2,title_1="Orignal",title_2="New Image"):
    plt.figure(figsize=(10,10))
    plt.subplot(1, 2, 1)
    plt.imshow(cv2.cvtColor(image_1, cv2.COLOR_BGR2RGB))
    plt.title(title_1)
    plt.subplot(1, 2, 2)
    plt.imshow(cv2.cvtColor(image_2, cv2.COLOR_BGR2RGB))
    plt.title(title_2)
    plt.show()
```

Spatial operations use the nanoring pixels to determine the present pixel value

### Linear Filtering

Filtering involves enhancing an image, for example removing the Noise from an image. Noise is caused by a bad camera or bad image compression. The same factors that cause noise may lead to blurry images, we can apply filters to sharpening these images. Convolution is a standard way to Filter an image the filter is called the kernel and different kernels perform different tasks. In addition, Convolution is used for many of the most advanced artificial intelligence algorithms. We simply take the dot product of the kernel and as an equally-sized portion of the image. We then shift the kernel and repeat.

Consider the following image:

```python
# Loads the image from the specified file
image = cv2.imread("lenna.png")
print(image)
# Converts the order of the color from BGR (Blue Green Red) to RGB (Red Green Blue) then renders the image from the array of data
plt.imshow(cv2.cvtColor(image, cv2.COLOR_BGR2RGB))
```

```
[[[125 137 226]
  [125 137 226]
  [133 137 223]
  ...
  [122 148 230]
  [110 130 221]
  [ 90  99 200]]

 [[125 137 226]
  [125 137 226]
  [133 137 223]
  ...
  [122 148 230]
  [110 130 221]
  [ 90  99 200]]

 [[125 137 226]
  [125 137 226]
  [133 137 223]
  ...
  [122 148 230]
  [110 130 221]
  [ 90  99 200]]

 ...

 [[ 60  18  84]
  [ 60  18  84]
  [ 58  27  92]
  ...
  [ 84  73 173]
  [ 76  68 172]
  [ 79  62 177]]

 [[ 57  22  82]
  [ 57  22  82]
  [ 62  32  96]
  ...
  [ 79  70 179]
  [ 81  71 181]
  [ 81  74 185]]

 [[ 57  22  82]
  [ 57  22  82]
  [ 62  32  96]
  ...
  [ 79  70 179]
  [ 81  71 181]
  [ 81  74 185]]]





<matplotlib.image.AxesImage at 0x7fd66435d950>




```

![png](../../.gitbook/assets/output\_17\_2)

The images we are working with are comprised of RGB values which are values from 0 to 255. Zero means white noise, this makes the image look grainy:

```python
# Get the number of rows and columns in the image
rows, cols,_= image.shape
# Creates values using a normal distribution with a mean of 0 and standard deviation of 15, the values are converted to unit8 which means the values are between 0 and 255
noise = np.random.normal(0,15,(rows,cols,3)).astype(np.uint8)
# Add the noise to the image
noisy_image = image + noise
# Plots the original image and the image with noise using the function defined at the top
plot_image(image, noisy_image, title_1="Orignal",title_2="Image Plus Noise")
```

![png](<../../.gitbook/assets/output\_19\_0 (1)>)

When adding noise to an image sometimes the value might be greater than 255, in this case, 256, is subtracted from the value to wrap the number around keeping it between 0 and 255. For example, consider an image with an RGB value of 137 and we add noise with an RGB value of 215 we get an RGB value of 352. We then subtract 256, the total number of possible values between 0 and 255, to get a number between 0 and 255.

#### Filtering Noise

Smoothing filters average out the Pixels within a neighborhood, they are sometimes called low pass filters. For mean filtering, the kernel simply averages out the kernels in a neighborhood.

```python
# Create a kernel which is a 6 by 6 array where each value is 1/36
kernel = np.ones((6,6))/36
```

The function `filter2D` performs 2D convolution between the image `src` and the `kernel` on each color channel independently. The parameter [ddepth](https://docs.opencv.org/master/d4/d86/group\_\_imgproc\_\_filter.html?utm\_medium=Exinfluencer\&utm\_source=Exinfluencer\&utm\_content=000026UJ\&utm\_term=10006555\&utm\_id=NA-SkillsNetwork-Channel-SkillsNetworkCoursesIBMDeveloperSkillsNetworkCV0101ENCoursera872-2022-01-01#filter\_depths) has to do with the size of the output image, we will set it to -1 so the input and output are the same size.

```python
# Filters the images using the kernel
image_filtered = cv2.filter2D(src=noisy_image, ddepth=-1, kernel=kernel)
```

We can plot the image before and after the filtering; we see the noise is reduced, but the image is blurry:

```python
# Plots the Filtered and Image with Noise using the function defined at the top
plot_image(image_filtered, noisy_image,title_1="Filtered image",title_2="Image Plus Noise")
```

![png](../../.gitbook/assets/output\_27\_0)

A smaller kernel keeps the image sharp, but filters less noise, here we try a 4x4 kernel

```python
# Creates a kernel which is a 4 by 4 array where each value is 1/16
kernel = np.ones((4,4))/16
# Filters the images using the kernel
image_filtered=cv2.filter2D(src=noisy_image,ddepth=-1,kernel=kernel)
# Plots the Filtered and Image with Noise using the function defined at the top
plot_image(image_filtered , noisy_image,title_1="filtered image",title_2="Image Plus Noise")
```

![png](<../../.gitbook/assets/output\_29\_0 (3)>)

#### Gaussian Blur

The function `GaussianBlur` convolves the source image with the specified Gaussian kernel It filters noise but does a better job of preserving the edges. It has the following parameters:

Parameters

`src` input image; the image can have any number of channels, which are processed independently

`ksize:` Gaussian kernel size

`sigmaX` Gaussian kernel standard deviation in the X direction

`sigmaY` Gaussian kernel standard deviation in the Y direction; if sigmaY is zero, it is set to be equal to sigmaX

```python
# Filters the images using GaussianBlur on the image with noise using a 4 by 4 kernel 
image_filtered = cv2.GaussianBlur(noisy_image,(5,5),sigmaX=4,sigmaY=4)
# Plots the Filtered Image then the Unfiltered Image with Noise
plot_image(image_filtered , noisy_image,title_1="Filtered image",title_2="Image Plus Noise")
```

![png](../../.gitbook/assets/output\_33\_0)

Sigma behaves like the size of the mean filter, a larger value of sigma will make the image blurry, but you are still constrained by the size of the filter, there we set sigma to 10

```python
# Filters the images using GaussianBlur on the image with noise using a 11 by 11 kernel 
image_filtered = cv2.GaussianBlur(noisy_image,(11,11),sigmaX=10,sigmaY=10)
# Plots the Filtered Image then the Unfiltered Image with Noise
plot_image(image_filtered , noisy_image,title_1="filtered image",title_2="Image Plus Noise")
```

![png](<../../.gitbook/assets/output\_35\_0 (1)>)

See what happens when you set different values of sigmaX, sigmaY, and or use non-square kernels.

#### Image Sharpening

Image Sharpening involves smoothing the image and calculating the derivatives. We can accomplish image sharpening by applying the following Kernel.

```python
# Common Kernel for image sharpening
kernel = np.array([[-1,-1,-1], 
                   [-1, 9,-1],
                   [-1,-1,-1]])
# Applys the sharpening filter using kernel on the original image without noise
sharpened = cv2.filter2D(image, -1, kernel)
# Plots the sharpened image and the original image without noise
plot_image(sharpened , image, title_1="Sharpened image",title_2="Image")
```

![png](<../../.gitbook/assets/output\_39\_0 (1)>)

### Edges

Edges are where pixel intensities change. The Gradient of a function outputs the rate of change; we can approximate the gradient of a grayscale image with convolution. There are several methods to approximate the gradient, let’s use the Sobel edge detector. This combines several convolutions and finding the magnitude of the result. Consider the following image:

```python
# Loads the image from the specified file
img_gray = cv2.imread('barbara.png', cv2.IMREAD_GRAYSCALE)
print(img_gray)
# Renders the image from the array of data, notice how it is 2 diemensional instead of 3 diemensional because it has no color
plt.imshow(img_gray ,cmap='gray')
```

```
[[181 201 202 ... 103 102  92]
 [171 198 201 ...  94  96  96]
 [175 195 193 ...  87  96  98]
 ...
 [100  97  97 ... 114 113 117]
 [ 94  97  99 ... 111 112 114]
 [ 96  95  98 ... 113 104 109]]





<matplotlib.image.AxesImage at 0x7fd6397c0210>




```

![png](../../.gitbook/assets/output\_42\_2)

We smooth the image, this decreases changes that may be caused by noise that would affect the gradient.

```python
# Filters the images using GaussianBlur on the image with noise using a 3 by 3 kernel 
img_gray = cv2.GaussianBlur(img_gray,(3,3),sigmaX=0.1,sigmaY=0.1)
# Renders the filtered image
plt.imshow(img_gray ,cmap='gray')
```

```
<matplotlib.image.AxesImage at 0x7fd63973a5d0>




```

![png](../../.gitbook/assets/output\_44\_1)

We can approximate the derivative in the X or Y direction using the `Sobel` function, here are the parameters:

`src`: input image

`ddepth`: output image depth, see combinations; in the case of 8-bit input images it will result in truncated derivatives

`dx`: order of the derivative x

`dx`: order of the derivative y

`ksize` size of the extended Sobel kernel; it must be 1, 3, 5, or 7

dx = 1 represents the derivative in the x-direction. The function approximates the derivative by convolving the image with the following kernel

\begin{bmatrix} 1 & 0 & -1 \\\ 2 & 0 & -2 \\\ 1 & 0 & -1 \end{bmatrix}

We can apply the function:

```python
ddepth = cv2.CV_16S
# Applys the filter on the image in the X direction
grad_x = cv2.Sobel(src=img_gray, ddepth=ddepth, dx=1, dy=0, ksize=3)
```

We can plot the result

```python
plt.imshow(grad_x,cmap='gray')
```

```
<matplotlib.image.AxesImage at 0x7fd648037050>




```

![png](../../.gitbook/assets/output\_52\_1)

dy=1 represents the derivative in the y-direction. The function approximates the derivative by convolving the image with the following kernel

\begin{bmatrix} \ \ 1 & \ \ 2 & \ \ 1 \\\ \ \ 0 & \ \ 0 & \ \ 0 \\\ -1 & -2 & -1 \end{bmatrix}

We can apply the function and plot the result

```python
# Applys the filter on the image in the X direction
grad_y = cv2.Sobel(src=img_gray, ddepth=ddepth, dx=0, dy=1, ksize=3)
plt.imshow(grad_y,cmap='gray')
```

```
<matplotlib.image.AxesImage at 0x7fd6396a8e90>




```

![png](../../.gitbook/assets/output\_56\_1)

We can approximate the gradient by calculating absolute values, and converts the result to 8-bit:

```python
# Converts the values back to a number between 0 and 255
abs_grad_x = cv2.convertScaleAbs(grad_x)
abs_grad_y = cv2.convertScaleAbs(grad_y)
```

Then apply the function `addWeighted` to calculates the sum of two arrays as follows:

```python
# Adds the derivative in the X and Y direction
grad = cv2.addWeighted(abs_grad_x, 0.5, abs_grad_y, 0.5, 0)
```

We then plot the results, we see the image with lines have high-intensity values representing a large gradient

```python
# Make the figure bigger and renders the image
plt.figure(figsize=(10,10))
plt.imshow(grad,cmap='gray')
```

```
<matplotlib.image.AxesImage at 0x7fd639636d50>




```

![png](../../.gitbook/assets/output\_62\_1)

### Median

Median filters find the median of all the pixels under the kernel area and the central element is replaced with this median value.

We can apply median filters to regular images but let’s see how we can use a median filter to improve segmentation. Consider the cameraman example

```python
# Load the camera man image
image = cv2.imread("cameraman.jpeg",cv2.IMREAD_GRAYSCALE)
# Make the image larger when it renders
plt.figure(figsize=(10,10))
# Renders the image
plt.imshow(image,cmap="gray")
```

```
<matplotlib.image.AxesImage at 0x7fd63968c810>




```

![png](../../.gitbook/assets/output\_65\_1)

Now let's apply a Median Filter by using the `medianBlur` function. The parameters for this function are `src`: The image and `ksize`: Kernel size

```python
# Filter the image using Median Blur with a kernel of size 5
filtered_image = cv2.medianBlur(image, 5)
# Make the image larger when it renders
plt.figure(figsize=(10,10))
# Renders the image
plt.imshow(filtered_image,cmap="gray")
```

```
<matplotlib.image.AxesImage at 0x7fd6481903d0>




```

![png](../../.gitbook/assets/output\_67\_1)

We would like to find the cameraman, but median filtering captures some of the background.

### Threshold Function Parameters

`src`: The image to use `thresh`: The threshold `maxval`: The maxval to use `type`: Type of filtering

The threshold function works by looking at each pixel's grayscale value and assigning a value if it is below the threshold and another value if it is above the threshold. In our example the threshold is 0 (black) and the type is binary inverse so if a value is above the threshold the assigned value is 0 (black) and if it is below or equals the threshold the maxval 255 (white) is used. So if the pixel is 0 black it is assigned 255 (white) and if the pixel is not black then it is assigned black which is what THRESH\_BINARY\_INV tells OpenCV to do. This is how it would work without THRESH\_OTSU.

Since we are using THRESH\_OTSU it means that OpenCV will decide an optimal threshold. In our example below the threshold, we provide does not get used in the filter OpenCV will use an optimal one.

```python
# Returns ret which is the threshold used and outs which is the image
ret, outs = cv2.threshold(src = image, thresh = 0, maxval = 255, type = cv2.THRESH_OTSU+cv2.THRESH_BINARY_INV)

# Make the image larger when it renders
plt.figure(figsize=(10,10))

# Render the image
plt.imshow(outs, cmap='gray')
```

```
<matplotlib.image.AxesImage at 0x7fd64808c4d0>




```

![png](../../.gitbook/assets/output\_69\_1)

Because those elements are mostly zeros the median filter will filter out accordingly:

###
