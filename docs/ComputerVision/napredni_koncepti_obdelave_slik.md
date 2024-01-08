# Advanced methods of image capture

## Hiperspectral imaging

Hiperspectral imaging is a method of capturing images that uses a lot of different spectrums of light. This allows us to get a lot of information about the object that we are imaging. For example we can use it to determine the chemical composition of the object. The usal digital camera is already doing multispectral image capturing (red, green, blue). Hiperspectral imaging is just an extension of that. For example the [sentinel 2](https://sentinel.esa.int/web/sentinel/missions/sentinel-2) satelite captures images in 13 different spectrums.

### How do we capture hiperspectral images

Making a hiperspectral image the passive way is not that hard. We just need to put a lot of different filters in front of the camera. The problem is that the more filters we put in front of the camera the less light gets to the camera. This means that we need to have a lot of light to get a good image. This is not a problem if we are imaging a static object, but if we are imaging a moving object we need to have a lot of light. This is why hiperspectral imaging is mostly used in static imaging. 

An alternative is to send specific wave length of light to the object and then measure the reflected light. This is called [spectroscopy](https://en.wikipedia.org/wiki/Spectroscopy). As we iterate through different bands of wavelengths we can measure the amount reflected for each band. From this data we can determine the material composition of the object. This technology is often combined with remote imaging for satellite imaging. There are libraries containing spectral reports of different materials. We can then compare the spectral report of the object that we are imaging with the spectral reports in the library to determine the material composition of the object.
- Spectroscopy: (study of the interaction between matter and electromagnetic radiation)
- Remote imaging: (imaging objects from a distance)

### How do we process hiperspectral images

Hiperspectral images are just a bunch of images in different spectrums. We can process them the same way we process normal images. We can use the same filters and the same algorithms. The only difference is that we have more data to work with. This means that we can get more information about the object that we are imaging.

- Whole pixel classification:
    find the closest spectral report in the library and classify the pixel as that material

- Subpixel classification:
    find the closest spectral report in the library and classify the pixel as that material. Then calculate the percentage of the pixel that is covered by that material.
    
using euclidean distance:
    $$ d = \sqrt{\sum_{i=1}^{n} (x_i - y_i)^2} $$ 
    where $x_i$ is the value of the pixel in the image and $y_i$ is the value of the pixel in the spectral report

using spectral angle mapper:
    $$ d = \arccos \left( \frac{\sum_{i=1}^{n} x_i y_i}{\sqrt{\sum_{i=1}^{n} x_i^2} \sqrt{\sum_{i=1}^{n} y_i^2}} \right) $$ 
    where $x_i$ is the value of the pixel in the image and $y_i$ is the value of the pixel in the spectral report

why spectral angle mapper is better than euclidean distance:
- euclidean distance is sensitive to noise
- spectral angle mapper is not sensitive to noise
- spectral angle mapper is not sensitive to illumination
    
Finding a percentage of the pixel that is covered by a material finding out which materials combine to make the pixel color.
Mixing of materials:
$$s(p)=\alpha_1s(e_1) + \alpha_2s(e_2) + ... + \alpha_n s(e_n)$$
where $s(p)$ is the spectral report of the pixel, $s(e_i)$ is the spectral report of the material $e_i$ and $\alpha_i$ is the percentage of the pixel that is covered by the material $e_i$.

By unmixing the pixel we can find out which materials are present in the pixel and what percentage of the pixel is covered by each material. To do this we create a matrix $A$ where each row is the spectral report of a material and each column is the percentage of the pixel that is covered by that material. We also create a vector $b$ where each element is the spectral report of the pixel. We then solve the following equation:

$$Ax = b$$

where $x$ is the vector of percentages of the pixel that is covered by each material.

### Spectral indexes

Spectral indexes are a way of extracting information from the images. They are calculated using the following formula:

$$SI = \frac{BAND_1 - BAND_2}{BAND_1 + BAND_2}$$

where $BAND_1$ and $BAND_2$ are the bands that we are using to calculate the spectral index. For example the NDVI spectral index is calculated using the following formula:

$$NDVI = \frac{NIR - RED}{NIR + RED}$$

where NIR is the near infrared band and RED is the red band.

### Processingg images from sentinel 2

The sentinel 2 satelite captures images in 13 different bands. The bands are in the following ranges:



## Thermal imaging

Thermal imaging is a method of capturing images that uses the infrared spectrum of light. Turns out when a object cools it radiates out energy in the infared spectrum. The hotter the object the more energy it radiates.