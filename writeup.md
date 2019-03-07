# **Finding Lane Lines on the Road** 

## A Short Writeup

Shuo SUN
ss1057532022@gmail.com

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Description of my pipeline.  
* are the steps that have been modified/added
a) convert the images to grayscale
b) apply Gaussian smoothing
c) apply canny edge detection to detect the edge of the objects
d) apply region masking to select the ROI
e) apply hough transform to identify line segments
*f) sort line segments into left/right lane by their position and slop in the image
*g) find the single best fitting line for left/right line segments
*h) draw the left/right single lane line on the image

In order to draw a single line on the left and right lanes, and minimise the disturbances caused by the shadows, radom line segments that are in the ROI, car body, etc, the algo needs to distinguish if a line segment belongs to the left lane or the right lane, or noise, I modified the draw_lines() function by 

a) for each line segment, if it's on the left side of the image, with a slop inclining towards right, it will be sorted to `left_lane_points`, vise versa. The rest of the line segaments will be regarded as noise
b) use `np.polyfit()` to find the best fitting line for all the `left/right_lane_points` 
c) draw the best fitting left/right lane line on the image


### 2. Potential shortcomings and issues with the current pipeline
based on its performance in the challenge.mp4, the current pipeline exibits the following issues: 

1.  Lanes are affected by the huge gap/boudary between two road segments

2.  Lanes are affected by the shadows

3.  The output lanes behave weirdly when entering the light color road segment

4.  The outline of the car body in the image may affect the edge detection

5.  The algo may not function properly when encountering a sharp turning as it uses straight lines to fit the lanes

### 3. Proposed improvements to the pipeline
1. Add a color selection to select only white and yellow lane lines to eliminate the influence of the gap, shadows, and road color, etc

2. Switch to use the weighted average slop and intercept of the line segments to fit the lane line (using the length of each line segment as its weight) instead of the current `np.polyfit()` method. _(Because that `np.polyfit()` only uses the starting and ending points of the line segment, disregarding the length of the line segments. When the available number of line segments sorted into left and right lanes is low, the `np.polyfit()` will produce weird fitting results.)_

3. Addjust the ROI to exclude the car body if the assumption that the camera position and FOV won't change in the long term is valid.

4. Limit the slop of the final fitted line
