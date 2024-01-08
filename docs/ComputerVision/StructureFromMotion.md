# Structure od a 3D scene given two different views of the scene

Lets say we have two images of the same scene taken from two different positions and both images contain a set of corresponding points. We may be able to calculate the position of the camera relative to the scene. This is called structure from motion. Knowing the position of the camera relative to the scene is useful for many applications. For example we can use it to calculate the distance to objects in the scene or to create a 3D model of the scene.

## Limitations of structure from motion

We can forget about reconstructing the scene if both images were taken in the same plane
In those cases we can only calculate homography between the two images.
### only translation

### only rotation

