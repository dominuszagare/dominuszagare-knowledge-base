# 3D reconstruction from 2D images

3D reconstruction is a proces of reconstructing the 3D structure of an object from a set of 2D images. The process is also known as **Structure from Motion** (SfM). The process is divided into two steps: **camera calibration** and **3D reconstruction**.

## point similarity
We have points in space $p_i$ and their projections on the image plane $p_{s,i}$. The points are related by the following equation:
$$
p_{s,i} = M p_i = (MH^{-1}_S)(H_S p_i)
$$
$ H_S $ is a homography matrix that maintains the similarity between the two planes. $ M $ is the camera projection matrix. 
$$ 
H_S = \begin{bmatrix} R' & t' & \\ 0 & \lambda  \end{bmatrix} => MH^{-1}_S = K[R|t] \begin{bmatrix} R'^T & -R'^Tt' & \\ 0 & \lambda  \end{bmatrix}
$$
$ K $ is the camera calibration matrix. $ R $ and $ t $ are the rotation and translation matrices respectively. $ R' $ and $ t' $ are the rotation and translation matrices of the second camera. $ \lambda $ is the scale factor.

## decompositn of the projection matrix
It is proven that the metric reconstruction of the 3D points is possible if the camera projection matrix is decomposed into the product of three matrices:
$$ H = H_S H_A H_P $$
where $H_S$ is similarity homografy $ H_A $ is the homography matrix of the affine transformation and $ H_P $ is the homography matrix of the perspective transformation. The affine transformation is a transformation that preserves the parallelism of the lines. The perspective transformation is a transformation that preserves the collinearity of the points. 

THis means we can take a multilayered approach to find the 3D points from uncalibrated camera 2D points.

1. Geting prespective 3D points from uncalibrated camera 2D points
2. Geting affine 3D points from prespective 3D points
3. Geting metric 3D points from affine 3D points

prequisites:
- we need to know the geometry of the object we are trying to reconstruct. 
- when transforming to perspective 3D points we need to know at least 3 points that are on a infinite plane.


## solving the projection matrix

Hhen solving prespective ith helps find the plane infinity. The plane infinity is the plane that contains the points at infinity. The points at infinity are the points that are parallel to the image plane. 

(ponorna tocka je tocka, ki je vzporedna z ravnino slike)
