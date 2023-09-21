---
Tags: 
Created: 2023-09-21 13:57:36
---
(Links:: [[Pervasive Computing]])
# Windowing
Many of the image processing operations can be selectively applied to a user defined region within the image. This region is known as the **region of interest**(**ROI**).
# Point processing
- When you change the brightness on your TV you change each pixel in the same way.
- These operations can be the result of applying an arithmetic function to the pixel
## Histogram
An image histogram plots how often each pixel value appears in the image against the value themselves.
![[histogram-4-1843420717.png|500]]
- **Histogram stretching**: 
  $f_1$ is the left-most non-zero bin in the histogram
  $f_2$ is the right-most non-zero bin in the histogram of the input image.$$g(x,y)=\frac{255}{f_{2}-f_{1}}\times(f(x,y)-f_{1})$$
- **Segmentation**: We can separate objects from the background with tools such as Thresholding or Edge detection
	- Thresholding: Any pixel brighter than a certain level should become white, all others darker than that level will become black -> high contrast black and white image
# Neighbourhood processing
- The value of a pixel in the output is determined by the value of the pixel at the same position in the input **and the neighbours**, together with a neighbourhood processing
- Use a small neighbourhood of a pixel in an input image to get a new brightness value in the output image
- **Correlation**: 
	- Correlation is an operation which works by scanning through the image and applying a mask to each pixel.
	- The mask is called kernel
	- The kernel is filled by numbers, not always equal to one—denoted *kernel coefficients*.
	- These coefficients weight the pixel value they are covering
	- The output of the correlation is a sum of weighted pixel values.
- Edge detection: Edges are local discontinuity in the pixel value that exceeds a given threshold
	- Edge detection can be obtained by a correlation with a kernel
# Morphological operations
Morphology is a branch of image processing which is particularly useful for analysing **shapes** in images.
- Kernels are **structuring elements** and contain 0's and 1's
  ![[SCR-20230921-tncf.png|500]]
- Structuring elements are applied using either a **Hit** or a 
# BLOB analysis

---
References: