# Gemetric trasfroms

## Geometric Operations and Other Mathematical Tools

### Objectives

In the first part of the lab, you will apply geometric transformations to an image. This allows you to perform different operations like reshape translation i.e. to shift, reshape and rotate the image. In the second part of the lab, you will learn how to apply some basic array and matrix operations to the image.

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
--2023-01-10 01:53:01--  https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/lenna.png
Resolving cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)... 169.63.118.104
Connecting to cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)|169.63.118.104|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 473831 (463K) [image/png]
Saving to: ‘lenna.png’

lenna.png           100%[===================>] 462.73K  --.-KB/s    in 0.009s  

2023-01-10 01:53:02 (49.4 MB/s) - ‘lenna.png’ saved [473831/473831]

--2023-01-10 01:53:03--  https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/baboon.png
Resolving cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)... 169.63.118.104
Connecting to cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)|169.63.118.104|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 637192 (622K) [image/png]
Saving to: ‘baboon.png’

baboon.png          100%[===================>] 622.26K  --.-KB/s    in 0.01s   

2023-01-10 01:53:03 (49.2 MB/s) - ‘baboon.png’ saved [637192/637192]

--2023-01-10 01:53:04--  https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-CV0101EN-SkillsNetwork/images%20/images_part_1/barbara.png
Resolving cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)... 169.63.118.104
Connecting to cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud (cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud)|169.63.118.104|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 185727 (181K) [image/png]
Saving to: ‘barbara.png’

barbara.png         100%[===================>] 181.37K  --.-KB/s    in 0.005s  

2023-01-10 01:53:04 (33.4 MB/s) - ‘barbara.png’ saved [185727/185727]
```

We will import the following:

```python
import matplotlib.pyplot as plt
import cv2
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

Geometric transformations allow you to perform different operations like translation i.e. to shift, reshape and rotate the image.

### Scaling

We can resize an image using the function `resize()` from `cv2` module for this purpose. You can specify the scaling factor or the size of the image:

Consider the following image with the corresponding intensity values:

```python
toy_image = np.zeros((6,6))
toy_image[1:5,1:5]=255
toy_image[2:4,2:4]=0
plt.imshow(toy_image,cmap='gray')
plt.show()
toy_image
```

![png](../../.gitbook/assets/output\_18\_0)

```
array([[  0.,   0.,   0.,   0.,   0.,   0.],
       [  0., 255., 255., 255., 255.,   0.],
       [  0., 255.,   0.,   0., 255.,   0.],
       [  0., 255.,   0.,   0., 255.,   0.],
       [  0., 255., 255., 255., 255.,   0.],
       [  0.,   0.,   0.,   0.,   0.,   0.]])
```

We can rescale along a specific axis:

* `fx`: scale factor along the horizontal axis
* `fy`: scale factor along the vertical axis

The parameter interpolation estimates pixel values based on neighboring pixels. `INTER_NEAREST` uses the nearest pixel and `INTER_CUBIC` uses several pixels near the pixel value we would like to estimate.

```python
new_toy = cv2.resize(toy_image,None,fx=2, fy=1, interpolation = cv2.INTER_NEAREST )
plt.imshow(new_toy,cmap='gray')
plt.show()

```

![png](../../.gitbook/assets/output\_21\_0)

Consider the following image:

```python
image = cv2.imread("lenna.png")
plt.imshow(cv2.cvtColor(image, cv2.COLOR_BGR2RGB))
plt.show()
```

![png](<../../.gitbook/assets/output\_23\_0 (1)>)

We can scale the horizontal axis by two and leave the vertical axis as is:

```python
new_image = cv2.resize(image, None, fx=2, fy=1, interpolation=cv2.INTER_CUBIC)
plt.imshow(cv2.cvtColor(new_image, cv2.COLOR_BGR2RGB))
plt.show()
print("old image shape:", image.shape, "new image shape:", new_image.shape)
```

![png](../../.gitbook/assets/output\_25\_0)

```
old image shape: (512, 512, 3) new image shape: (512, 1024, 3)
```

In the same manner, we can scale the vertical axis by two:

```python
new_image = cv2.resize(image, None, fx=1, fy=2, interpolation=cv2.INTER_CUBIC)
plt.imshow(cv2.cvtColor(new_image, cv2.COLOR_BGR2RGB))
plt.show()
print("old image shape:", image.shape, "new image shape:", new_image.shape)
```

![png](<../../.gitbook/assets/output\_27\_0 (1)>)

```
old image shape: (512, 512, 3) new image shape: (1024, 512, 3)
```

We can scale the horizontal axis and vertical axis by two.

```python
new_image = cv2.resize(image, None, fx=2, fy=2, interpolation=cv2.INTER_CUBIC)
plt.imshow(cv2.cvtColor(new_image, cv2.COLOR_BGR2RGB))
plt.show()
print("old image shape:", image.shape, "new image shape:", new_image.shape)
```

![png](<../../.gitbook/assets/output\_29\_0 (1)>)

```
old image shape: (512, 512, 3) new image shape: (1024, 1024, 3)
```

We can also shrink the image by setting the scaling factor to a real number between 0 and 1:

```python
new_image = cv2.resize(image, None, fx=1, fy=0.5, interpolation=cv2.INTER_CUBIC)
plt.imshow(cv2.cvtColor(new_image, cv2.COLOR_BGR2RGB))
plt.show()
print("old image shape:", image.shape, "new image shape:", new_image.shape)
```

![png](../../.gitbook/assets/output\_31\_0)

```
old image shape: (512, 512, 3) new image shape: (256, 512, 3)
```

We can also specify the number of rows and columns:

```python
rows = 100
cols = 200
```

```python
new_image = cv2.resize(image, (100, 200), interpolation=cv2.INTER_CUBIC)
plt.imshow(cv2.cvtColor(new_image, cv2.COLOR_BGR2RGB))
plt.show()
print("old image shape:", image.shape, "new image shape:", new_image.shape)
```

![png](../../.gitbook/assets/output\_34\_0)

```
old image shape: (512, 512, 3) new image shape: (200, 100, 3)
```

### Translation

Translation is when you shift the location of the image. `tx` is the number of pixels you shift the location in the horizontal direction and `ty` is the number of pixels you shift in the vertical direction. You can create the transformation matrix $M$ to shift the image.

In this example, we shift the image 100 pixels horizontally:

```python
tx = 100
ty = 0
M = np.float32([[1, 0, tx], [0, 1, ty]])
M
```

```
array([[  1.,   0., 100.],
       [  0.,   1.,   0.]], dtype=float32)
```

The shape of the image is given by:

```python
rows, cols, _ = image.shape
```

We use the function `warpAffine` from the `cv2` module. The first input parater is an image array, the second input parameter is the transformation matrix `M`, and the final input paramter is the length and width of the output image $(cols,rows)$:

```python
new_image = cv2.warpAffine(image, M, (cols, rows))
```

We can plot the image; the portions of the image that do not have any intensities are set to zero:

```python
plt.imshow(cv2.cvtColor(new_image, cv2.COLOR_BGR2RGB))
plt.show()
```

![png](<../../.gitbook/assets/output\_43\_0 (1)>)

We can see some of the original image has been cut off. We can fix this by changing the output image size: `(cols + tx,rows + ty)`:

```python
new_image = cv2.warpAffine(image, M, (cols + tx, rows + ty))
plt.imshow(cv2.cvtColor(new_image, cv2.COLOR_BGR2RGB))
plt.show()
```

![png](../../.gitbook/assets/output\_45\_0)

We can shift the image horizontally:

```python
tx = 0
ty = 50
M = np.float32([[1, 0, tx], [0, 1, ty]])
new_iamge = cv2.warpAffine(image, M, (cols + tx, rows + ty))
plt.imshow(cv2.cvtColor(new_iamge, cv2.COLOR_BGR2RGB))
plt.show()
```

![png](../../.gitbook/assets/output\_47\_0)

### Rotation

We can rotate an image by angle θ which is achieved by the Rotation Matrix `getRotationMatrix2D`.

`center`: Center of the rotation in the source image. We will only use the center of the image.

`angle`: Rotation angle in degrees. Positive values mean counter-clockwise rotation (the coordinate origin is assumed to be the top-left corner).

`scale`: Isotropic scale factor, in this course the value will be one.

We can rotate our toy image by 45 degrees:

```python
theta = 45.0
M = cv2.getRotationMatrix2D(center=(3, 3), angle=theta, scale=1)
new_toy_image = cv2.warpAffine(toy_image, M, (6, 6))
```

```python
plot_image(toy_image, new_toy_image, title_1="Orignal", title_2="rotated image")
```

![png](<../../.gitbook/assets/output\_53\_0 (2)>)

Looking at intensity values, we see that many values have been interpolated:

```python
new_toy_image 
```

```
array([[  0.        ,   0.        ,  28.38867188,   0.        ,
          0.        ,   0.        ],
       [  0.        ,  47.8125    , 223.125     , 151.40625   ,
          0.        ,   0.        ],
       [ 47.8125    , 223.125     , 103.59375   , 183.28125   ,
        151.40625   ,   0.        ],
       [195.234375  , 165.10253906,   0.        ,   0.        ,
        234.82910156,  89.89746094],
       [ 47.8125    , 223.125     , 103.59375   , 183.28125   ,
        151.40625   ,   0.        ],
       [  0.        ,  47.8125    , 223.125     , 151.40625   ,
          0.        ,   0.        ]])
```

We can perform the same operation on color images:

```python
cols, rows, _ = image.shape
```

```python
M = cv2.getRotationMatrix2D(center=(cols // 2 - 1, rows // 2 - 1), angle=theta, scale=1)
new_image = cv2.warpAffine(image, M, (cols, rows))
```

```python
plt.imshow(cv2.cvtColor(new_image, cv2.COLOR_BGR2RGB))
plt.show()
```

![png](../../.gitbook/assets/output\_59\_0)

## Mathematical Operations

### Array Operations

We can perform array operations on an image; Using Python broadcasting, we can add a constant to each pixel's intensity value.

```python
new_image = image + 20

plt.imshow(cv2.cvtColor(new_image, cv2.COLOR_BGR2RGB))
plt.show()
```

![png](../../.gitbook/assets/output\_63\_0)

We can also multiply every pixel's intensity value by a constant value.

```python
new_image = 10 * image
plt.imshow(cv2.cvtColor(new_image, cv2.COLOR_BGR2RGB))
plt.show()
```

![png](../../.gitbook/assets/output\_65\_0)

We can add the elements of two arrays of equal shape. In this example, we generate an array of random noises with the same shape and data type as our image.

```python
Noise = np.random.normal(0, 20, (rows, cols, 3)).astype(np.uint8)
Noise.shape

```

```
(512, 512, 3)
```

We add the generated noise to the image and plot the result. We see the values that have corrupted the image:

```python
new_image = image + Noise

plt.imshow(cv2.cvtColor(new_image, cv2.COLOR_BGR2RGB))
plt.show()
```

![png](<../../.gitbook/assets/output\_69\_0 (2)>)

At the same time, we can multiply the elements of two arrays of equal shape. We can multiply the random image and the Lenna image and plot the result.

```python
new_image = image*Noise

plt.imshow(cv2.cvtColor(new_image, cv2.COLOR_BGR2RGB))
plt.show()
```

![png](../../.gitbook/assets/output\_71\_0)

### Matrix Operations

Grayscale images are matrices. Consider the following grayscale image:

```python
im_gray = cv2.imread('barbara.png', cv2.IMREAD_GRAYSCALE)
im_gray.shape

plt.imshow(im_gray,cmap='gray')
plt.show()
```

![png](../../.gitbook/assets/output\_74\_0)

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

We can plot the matrix `U` and `V`:

```python
 plot_image(U,V,title_1="Matrix U ",title_2="matrix  V")
```

![png](../../.gitbook/assets/output\_82\_0)

We see most of the elements in `S` are zero:

```python
plt.imshow(S,cmap='gray')
plt.show()
```

![png](../../.gitbook/assets/output\_84\_0)

We can find the matrix product of all the matrices. First, we can perform matrix multiplication on `S` and `U` and assign it to `B` and plot the results:

```python
B = S.dot(V)
plt.imshow(B,cmap='gray')
plt.show()
```

![png](../../.gitbook/assets/output\_86\_0)

We can find the matrix product of `U`, `S`, and `B`. We see it’s the entire image:

```python
A = U.dot(B)
```

```python
plt.imshow(A,cmap='gray')
plt.show()
```

It turns out many elements are redundant, so we can eliminate some rows and columns of `S` and `V` and approximate the image by finding the product.

```python
for n_component in [1,10,100,200, 500]:
    S_new = S[:, :n_component]
    V_new = V[:n_component, :]
    A = U.dot(S_new.dot(V_new))
    plt.imshow(A,cmap='gray')
    plt.title("Number of Components:"+str(n_component))
    plt.show()
```

We see we only need 100 to 200 Components to represent the image.

###
