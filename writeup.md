
**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

[image1]: ./test_images_output/processedsolidWhiteRight.jpg
[image2]: ./test_images_output/processedsolidWhiteRight.jpg
[image3]: ./test_images_output/processedsolidYellowCurve.jpg
[image4]: ./test_images_output/processedsolidYellowCurve2.jpg
[image5]: ./test_images_output/processedsolidYellowLeft.jpg
[image6]: ./test_images_output/processedwhiteCarLaneSwitch.jpg

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. 

1) Converted the images to grayscale in order to normalize the colors
2) Applied gaussian blur to the image with a kernel size of 5 to reduce noise
3) Applied canny edge detection with a low threshold of 50 and a high threshold of 150
4) Built a mask of the region of interest using a polygon
5) Applied hough transform with a threshold of 15, min line length of 40 and maximum line gap of 20.
6) Blended the result of step 5 with the original image 

In order to draw a single line on the left and right lanes, I modified the draw_lines() to do the follwogin:

1) separate left and right lines by their slope
2) take all collected points and average them by finding the fit line coefficients using polyfit
3) used numpy.poly1d() to represent the line fitting fn from step 2, to be used to extrapolate the lines
4) draw left and right lines using the fn from step 3. I used 0 and 960 to extrapolate the left and right lines.

#### The processed images
![alt text][image1]
![alt text][image2]
![alt text][image3]
![alt text][image4]
![alt text][image5]
![alt text][image6]

#### The processed videos
- [solid_white_right_video](https://youtu.be/qh0XiLDBGRU)
- [solid_yellow_left_video](https://youtu.be/qh0XiLDBGRU)

### 2. Identify potential shortcomings with your current pipeline

The 2 main problems I see with this implementation are:

1) After applying grayscale, the solid yellow left line blends in with the concrete when there's a shadow. This throws off the detection algorithm as seen in the solid yellow video 
3) The extrapolation algorithm only work well with straight lines. This is why when it encounters a line that is not straight you can see it failt to find the lines in the challenge video


### 3. Suggest possible improvements to your pipeline

To address the problems in the previous section we can:
1) Use opencv inRange to find the yellow pixels in the area of interest before applying grayscale, then use the resulting image in the pipeline 
2) I am not 100% sure about this one but maybe we can use the polyfit fn to fit 2nd or 3rd degree polinomials to find curved lines. 