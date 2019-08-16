# **Finding Lane Lines on the Road** 

*Can Cildan - August 2019*

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report



[//]: # (Image References)

[image11]: ./test_images_output/1_gray.jpg "Grayscale"
[image12]: ./test_images_output/2_gaussian_blur.jpg "Gaussian Blur"
[image13]: ./test_images_output/3_canny.jpg "Canny Edge Detection"
[image14]: ./test_images_output/4_region_of_interest.jpg "Region of Interest"
[image15]: ./test_images_output/5_hough.jpg "Hough Transform"
[image16]: ./test_images_output/6_weighted.jpg "Weighted"
[image17]: ./test_images_output/7_improved_hough.jpg "Improved Hough Transform"

[image21]: ./test_images_output/pipeline_solidWhiteCurve.jpg "solidWhiteCurve"
[image22]: ./test_images_output/pipeline_solidWhiteRight.jpg "solidWhiteRight"
[image23]: ./test_images_output/pipeline_solidYellowCurve.jpg "solidYellowCurve"
[image24]: ./test_images_output/pipeline_solidYellowCurve2.jpg "solidYellowCurve2"
[image25]: ./test_images_output/pipeline_solidYellowLeft.jpg "solidYellowLeft"
[image26]: ./test_images_output/pipeline_whiteCarLaneSwitch.jpg "whiteCarLaneSwitch"


[image31]: ./test_images_output/pipeline_improved_solidWhiteCurve.jpg "improved_solidWhiteCurve"
[image32]: ./test_images_output/pipeline_improved_solidWhiteRight.jpg "improved_solidWhiteRight"
[image33]: ./test_images_output/pipeline_improved_solidYellowCurve.jpg "improved_solidYellowCurve"
[image34]: ./test_images_output/pipeline_improved_solidYellowCurve2.jpg "improved_solidYellowCurve2"
[image35]: ./test_images_output/pipeline_improved_solidYellowLeft.jpg "improved_solidYellowLeft"
[image36]: ./test_images_output/pipeline_improved_whiteCarLaneSwitch.jpg "improved_whiteCarLaneSwitch"



---

### Reflection

### 1. Pipeline Description:

My pipeline consisted of 6 steps which can be seen below. I started with some preprocessing beforehand and then applied primary lane detection approachs as Canny Edge Detection and Hough Transformation.

The detail of pipeline execution order is:
* 1 - Grayscale transform
* 2 - Gaussian smoothing
* 3 - Canny Edge detection
* 4 - Find the region of interest & Mask the Region of Interest
* 5 - Apply Hough transform
* 6 - Weighted images

##### 1 - Grayscale Tranformation
The first step of the pipeline is to convert the image to grayscale. I applied`cv2.cvtColor(img, cv2.COLOR_RGB2GRAY)`.

![alt text][image11] 

##### 2 - Gaussian smoothing
Then applied 
Gaussian blur function `cv2.GaussianBlur()` smoothening out the noise in the image with the  kernel value 5.

![alt text][image12]

##### 3 - Canny Edge detection
As third step, I applied  Canny's Edge Detection algorithm find possible edges  by extracting the edges in an image using `cv2.Canny()`. I used 50 as the low threshold and 150 as high threshold.

![alt text][image13]

##### 4 - Find the region of interest & Mask the Region of Interest
After Canny Edge Detection applied, there are more straight edges other than the lane lines. Therefore these unwanted edges should be filtered away by using Region of Intrest masking.

![alt text][image14]

##### 5 - Apply Hough transform
Now, we can see the lane boundary of our interest. I applied Hough Transformation to approximate lines by using `cv2.HoughLinesP()` with the below parameters:
`rho = 1`
`theta = np.pi / 180`
`threshold = 35`
`min_line_len = 10` 
`max_line_gap = 25`

![alt text][image15]

In order to draw a single line on the left and right lanes, I created new function as draw_lines_improved() function by applying below steps:
* Any lines with slopes 0 and infinite are discarded.
* Assuming the line touches the bottom end of the image.
* Average values calculated therefore only a single line is plotted for left and right lane boundary.

![alt text][image17]

##### 6 - Weighted images
As a final step I merged Hough Transformed image and the original image by using `cv2.addWeighted()`.

![alt text][image16]

### 2. Potential Shortcomings with my current pipeline:

* It is not succesfully work when the lane in the road is curving (challenge video).
* It assumed that lane lines end to the bottom, while this hardcoded masking would not be true for all conditions (challenge video).
* Video and picture have good lighting conditions and don't have much noises which would we face in real world, conditions like shadow, rain, snow, low light, etc.
* Processing is quite slow to use in a real world self driving car.


### 3. Suggest possible improvements to my pipeline:

* Algorithm should detect curves while any road has curves at many point.
* Region of intrest area should be widened to detect other vehicles' drive line.
* A memory would be created for the pipeline, if no line segments are detected, algorithm could continue to overlay the last calculated line until another is found.