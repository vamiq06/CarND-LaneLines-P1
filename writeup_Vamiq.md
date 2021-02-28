

###Finding Lane Lines on the Road


Reflection:

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of the following steps:
1. Read the image using cv2.imread
2. Convertted teh image to grayscale using the helper function provided
3. Applied a gaussian blur with a kernel size of 3 using teh provided helper function
4. Applying canny edge detection in many combinations so that all the edges represnting the lanes are visible
5. Define vertices of a polygon such that the polygon represents the lanes(region of interest) on the picture. Then use the region_of_interest() helper function to mask the canny edge. inside the polygon
6. Run the Hough transform using cv2.HoughLinesP() function and determine the extremities of the hough lines identified in the masked region
7. Apply an averaging filter using the draw_lines() function and apply it to every frame of the video
8. Draw teh line calculated by the average filter on top of the original picture

The initial code with helper ucntions was drawign every hough line identified by the HoughLinesP() command.In order to draw a single line on the left and right lanes, I modified the draw_lines() function by doing the following:
1. Separate the lines relatign to the right and left lane using the difference in their slope(slope>0 - right and slope<0 - left)
2. Average the extreme points of all the lines identified
3. Find the slope of the line formed using this averaged points for each lane.
4. calculate the y-intercept of each of these lines which act as a lane
5. Calculate the extreme points in the lane by determing teh x-coordinates based on the Y-axis limits.
6. With the bounds of the lanes identified, draw the lines which represent the lanes


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when there is a curve in the road. We may not be able to accurately represent lanes using straight lines in this case. 

Another shortcoming could be identification of lanes near intersections. Many intersections have a very curved lane guiding the vehicles

Third shortcoming is the fixed region of interest we have throughout the pipeline. This could be problematic if the car is going uphill or downhill. It may not be able to detect the lanes if they move out of the scope of the region of interest

Another shortcoming is the detection of canny edges using different thresholds. In the pipline we use a fixed threshold which may be a problem depending on the external lighting.


### 3. Suggest possible improvements to your pipeline

An improvement of the first shortcoming could be using better fit  methods(2nd degree or 3rd degree fit) to represent the lanes instead of 1D line fit.

The second short  coming can also benefit from using a higher degree fit

For the third shortcoming, we may be able to change the region of interest based on the orientation of the vehicle. This can be done using an input from a gyroscope. Thus giving better selection of regions to mask the image

For the canny detection using thresholds. we may be able to use different thresholds in different lighting conditions based on the inpur from light sensors