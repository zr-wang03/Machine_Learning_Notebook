# Gemetric trasfroms

## Geometric Operations and Other Mathematical Tools

### Objectives

In the first part of the lab, you will apply geometric transformations to an image. This allows you to perform different operations like reshape translation, i.e. to shift, reshape and rotate the image. In the second part of the lab, you will learn how to apply some basic array and matrix operations to the image.

* Geometric Operations
  * Scaling
  * Translation
  * Rotation
* Mathematical Operations
  * Array Operations
  * Matix Operations n

***

Download the image for the lab:

```python
!wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/lenna.png -O lenna.png
!wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/baboon.png -O baboon.png
!wget https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/barbara.png -O barbara.png  
```

```
--2023-01-09 16:32:50--  https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/lenna.png
Resolving cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)... 169.63.118.104
Connecting to cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)|169.63.118.104|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 473831 (463K) [image/png]
Saving to: ‘lenna.png’

lenna.png           100%[===================>] 462.73K  --.-KB/s    in 0.01s   

2023-01-09 16:32:50 (38.6 MB/s) - ‘lenna.png’ saved [473831/473831]

--2023-01-09 16:32:51--  https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/baboon.png
Resolving cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)... 169.63.118.104
Connecting to cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)|169.63.118.104|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 637192 (622K) [image/png]
Saving to: ‘baboon.png’

baboon.png          100%[===================>] 622.26K  --.-KB/s    in 0.01s   

2023-01-09 16:32:51 (46.9 MB/s) - ‘baboon.png’ saved [637192/637192]

--2023-01-09 16:32:52--  https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/barbara.png
Resolving cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)... 169.63.118.104
Connecting to cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)|169.63.118.104|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 185727 (181K) [image/png]
Saving to: ‘barbara.png’

barbara.png         100%[===================>] 181.37K  --.-KB/s    in 0.005s  

2023-01-09 16:32:52 (37.8 MB/s) - ‘barbara.png’ saved [185727/185727]
```

We will be using the following imported functions in this lab:

```python
import matplotlib.pyplot as plt
from PIL import Image
import numpy as np
```

First, let's define a helper function to plot two images side-by-side. You will not need to understand this code this moment, but this function will be used repeatedly in this tutorial to showcase the results.

```python
def plot_image(image_1, image_2,title_1="Orignal",title_2="New Image"):
    plt.figure(figsize=(10,10))
    plt.subplot(1, 2, 1)
    plt.imshow(image_1,cmap="gray")
    plt.title(title_1)
    plt.subplot(1, 2, 2)
    plt.imshow(image_2,cmap="gray")
    plt.title(title_2)
    plt.show()
```

## Geometric Transformations

Geometric transformations allow you to perform different operations like translation, i.e. to shift, reshape and rotate the image.

We can resize an image using the method `resize()` of `PIL` images, which takes the resized image's `width` and `height` as paramters.

Consider the following image:

```python
image = Image.open("lenna.png")
plt.imshow(image)
plt.show()
```

![png](../../.gitbook/assets/output\_16\_0)

We can scale the horizontal axis by two and leave the vertical axis as is:

```python
width, height = image.size
new_width = 2 * width
new_hight = height
new_image = image.resize((new_width, new_hight))
plt.imshow(new_image)
plt.show()
```

![png](<../../.gitbook/assets/output\_18\_0 (1)>)

In the same manner, we can scale the vertical axis by two:

```python
new_width = width
new_hight = 2 * height
new_image = image.resize((new_width, new_hight))
plt.imshow(new_image)
plt.show()
```

![png](../../.gitbook/assets/output\_20\_0)

We can double both the width and the height of the image:

```python
new_width = 2 * width
new_hight = 2 * height
new_image = image.resize((new_width, new_hight))
plt.imshow(new_image)
plt.show()
```

![png](../../.gitbook/assets/output\_22\_0)

We can also shrink the image's width and height both by 1/2:

```python
new_width = width // 2
new_hight = height // 2

new_image = image.resize((new_width, new_hight))
plt.imshow(new_image)
plt.show()
```

![png](../../.gitbook/assets/output\_24\_0)

### Rotation

We can rotate an image by angle $\theta$, using the method `rotate`.

We can rotate our toy image by 45 degrees:

```python
theta = 45
new_image = image.rotate(theta)
```

```python
plt.imshow(new_image)
plt.show()
```

![png](<../../.gitbook/assets/output\_29\_0 (4)>)

## Mathematical Operations

### Array Operations

We can perform array operations on an image; Using Python broadcasting, we can add a constant to each pixel's intensity value.

Before doing that, we must first we convert the PIL image to a numpy array.

```python
image = np.array(image)
```

We can then add the constant to the image array:

```python
new_image = image + 20
plt.imshow(new_image)
plt.show()
```

![png](<../../.gitbook/assets/output\_35\_0 (2)>)

We can also multiply every pixel's intensity value by a constant value.

```python
new_image = 10 * image
plt.imshow(new_image)
plt.show()
```

![png](<../../.gitbook/assets/output\_37\_0 (1)>)

We can add the elements of two arrays of equal shape. In this example, we generate an array of random noises with the same shape and data type as our image.

```python
Noise = np.random.normal(0, 20, (height, width, 3)).astype(np.uint8)
Noise.shape
```

```
(512, 512, 3)
```

We add the generated noise to the image and plot the result. We see the values that have corrupted the image:

```python
new_image = image + Noise

plt.imshow(new_image)
plt.show()
```

![png](../../.gitbook/assets/output\_41\_0)

At the same time, we can multiply the elements of two arrays of equal shape. We can multiply the random image and the Lenna image and plot the result.

```python
new_image = image*Noise

plt.imshow(new_image)
plt.show()
```

![png](../../.gitbook/assets/output\_43\_0)

### Matrix Operations

Grayscale images are matrices. Consider the following grayscale image:

```python
im_gray = Image.open("barbara.png")
```

Even though the image is gray, it has three channels; we can convert it to a one-channel image.

```python
from PIL import ImageOps 
```

```python
im_gray = ImageOps.grayscale(im_gray) 
```

We can convert the PIL image to a numpy array:

```python
im_gray = np.array(im_gray )
```

```python
plt.imshow(im_gray,cmap='gray')
plt.show()
```

![png](../../.gitbook/assets/output\_52\_0)

We can apply algorithms designed for matrices. We can use Singular Value Decomposition, decomposing our image matrix into a product of three matrices.

```python
U, s, V = np.linalg.svd(im_gray , full_matrices=True)
```

We see `s` is not rectangular:

```python
s.shape
```

```
(512,)
```

We can convert `s` to a diagonal matrix `S`:

```python
S = np.zeros((im_gray.shape[0], im_gray.shape[1]))
S[:image.shape[0], :image.shape[0]] = np.diag(s)
```

We can plot the matrix U and V:

```python
plot_image(U, V, title_1="Matrix U", title_2="Matrix V")
```

![png](../../.gitbook/assets/output\_60\_0)

We see most of the elements in S are zero:

```python
plt.imshow(S, cmap='gray')
plt.show()
```

![png](../../.gitbook/assets/output\_62\_0)

We can find the matrix product of all the matrices. First, we can perform matrix multiplication on S and U and assign it to `B` and plot the results:

```python
B = S.dot(V)
plt.imshow(B,cmap='gray')
plt.show()
```

![png](../../.gitbook/assets/output\_64\_0)

We can find the matrix product of `U`, `S`, and `B`. We see it's the entire image:

```python
A = U.dot(B)
```

```python
plt.imshow(A,cmap='gray')
plt.show()
```

![png](../../.gitbook/assets/output\_67\_0)

It turns out that many elements are redundant. We can eliminate some rows and columns of S and V and approximate the image by finding the product:

```python
for n_component in [1,10,100,200, 500]:
    S_new = S[:, :n_component]
    V_new = V[:n_component, :]
    A = U.dot(S_new.dot(V_new))
    plt.imshow(A,cmap='gray')
    plt.title("Number of Components:"+str(n_component))
    plt.show()
```

![png](../../.gitbook/assets/output\_69\_0)

![png](<../../.gitbook/assets/output\_69\_1 (1)>)

![png](../../.gitbook/assets/output\_69\_2)

![png](../../.gitbook/assets/output\_69\_3)

![png](../../.gitbook/assets/output\_69\_4)

We see we only need 100 to 200 Components to represent the image.

###
