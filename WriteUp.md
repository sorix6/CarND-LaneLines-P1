# **Finding Lane Lines on the Road** 



**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 7 steps. 

1. Check if the input image is of standard size (960x540), otherwise, resize it
2. Turn the image to grayscale
3. Apply a Gaussian blur to the grayscale image to reduce noise. The value of the Kernel size has been determined through trial and error tests
4. Apply a Canny filter on the grayscale image to find the edges. Low and high treshholds are dynamically computed for each picture
5. Define a region of interest in order to ignore edges that are not in areas where we expect lanes to be
 - the region of interest has been defined in the form of a trapezoid:
     - bottom corners in the bottom corners of the image
     - top corners, having an Y value of 50 pixels + half of the image height and X values, 20 pixels appart, in the center of the image
6. Apply the Hough transform in order to find the line segments corresponding to the lanes. Parameters have been selected through try and error tests
7. Extrapolate the lines found through the Hough transform in order to get the two lanes:
 - for each line, determine if the slope is positive (left lane) or negative (right lane)

 - lanes with the following features are ignored:
   - the difference between the X axis values for the start and the end point is larger than 1/3 or the picture
   - the difference between the X axis values for the start and the end point is smaller than 15 (vertical and almost lignes)
   - lines which have a slope with the absolute value smaller than 0.5

 - for all valid lines, the slope, intercept and weight (the length of the line) are added to arrays on the corresponding side

 - a weighted average for the slope and intercept is computed for each side

 - we choose the start and an end point for our extrapolated lines for each side
   - the start point will have the Y value at the bottom side of the image 
   - the end point will have the Y value at the end of the region of interest
   - the X values will be computed by using the line function for each side: y = slope * x + intercept

### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be the poor detection in areas with bright light, too much shade or sudden passes from one of these areas to another. Lanes are much less visible and as seen in the Challenge video, in these areas, the lanes are not well detected.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to add color detection on RGB images for white and yellow and to apply other edge detection algorithms, like Sobel.
