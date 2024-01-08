# Affine image alignment

## Image aligment

Image aligment is a problem of finding the image transformation that wil minimize the difference between two images. The image aligment is used in many computer vision applications like panorama stitching, object tracking, object recognition, etc.
[Image transformations refresher](https://www.youtube.com/watch?v=B8kMB6Hv2eI)

## Image aligment using affine transformation

[Example template matching](https://www.youtube.com/watch?v=1_hwFc8PXVE)
The affine transformation is a transformation that preserves the parallelism of the lines. This means that the affine transformation can be used to transform the image. The affine transformation is defined using the following formula:

$$ 
\begin{bmatrix}
x' \\
y' \\
1
\end{bmatrix} = \begin{bmatrix}
a_{11} & a_{12} & t_x \\
a_{21} & a_{22} & t_y \\
0 & 0 & 1
\end{bmatrix} \begin{bmatrix}
x \\
y \\
1
\end{bmatrix}
$$

where $x$ and $y$ are the coordinates of the pixel in the original image, $x'$ and $y'$ are the coordinates of the pixel in the transformed image, $a_{11}$ and $a_{12}$ are the parameters of the transformation, $a_{21}$ and $a_{22}$ are the parameters of the transformation, $t_x$ and $t_y$ are the parameters of the transformation.

Error function:

$$ E = \sum_{i=1}^N \left( \left( a_{11} x_i + a_{12} y_i + t_x - x'_i \right)^2 + \left( a_{21} x_i + a_{22} y_i + t_y - y'_i \right)^2 \right) $$

where $x_i$ and $y_i$ are the coordinates of the pixel in the original image, $x'_i$ and $y'_i$ are the coordinates of the pixel in the transformed image.

## Rigid image alignment in frequency domain

There exists a solution to image aligment that dosent need iterative or optimization methods. It is based on the fact that the Fourier transform of the image is a complex number that has a magnitude and a phase. The magnitude of the Fourier transform of the image is the same for all images of the same object. The phase of the Fourier transform of the image is different for different images of the same object. This means that we can use the phase of the Fourier transform of the image to find the transformation that will align the images. The rigid image alignment in frequency domain is defined by the following equation:

$$ 
\begin{bmatrix}
x' \\
y' \\
1
\end{bmatrix} = \begin{bmatrix}
\cos \theta & -\sin \theta & t_x \\
\sin \theta & \cos \theta & t_y \\
0 & 0 & 1
\end{bmatrix} \begin{bmatrix}
x \\
y \\
1
\end{bmatrix}
$$

where $x$ and $y$ are the coordinates of the pixel in the original image, $x'$ and $y'$ are the coordinates of the pixel in the transformed image, $\theta$ is the angle of the transformation, $t_x$ and $t_y$ are the parameters of the transformation.

2D fast fourier transform:

$$ F(u,v) = \sum_{x=0}^{M-1} \sum_{y=0}^{N-1} f(x,y) e^{-j2\pi \left( \frac{ux}{M} + \frac{vy}{N} \right)} $$

where $F(u,v)$ is the Fourier transform of the image, $f(x,y)$ is the image, $M$ is the width of the image, $N$ is the height of the image, $u$ is the horizontal frequency, $v$ is the vertical frequency.
