# **Finding Lane Lines on the Road** 

Author: Sebastian Brannstrom

## Project description

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 9 steps. 

1. Read frame from video into still image
1. Convert image to grayscale
1. Blur image slightly to reduce noise
1. Run Canny edge detection
1. Cut out area of interest based on camera viewpoint
1. Apply Hough line detection
1. Group line segments based on angle and position into "extended lines"
1. Collect extended lines over N frames
1. Draw the maximum extended line from the set of N lines

The best Canny edge detected was produced with a blur `kernel_size=11` and then applying a `low_threshold=100` and `high_threshold=200`. This seems to work alright for yellow lines as well.

The Hough transform parameters were tricky to get right, but `rho=3`, `theta=3*np.pi/180`, `threshold=30`, `min_line_length=100` and `max_line_gap=50` work alright, except that it sometimes believes white cars to be part of the lane lines.

The lines created by this process generally track segments of lane markings, but as the desired end result is a solid boundary line no matter the lane pattern, we need to aggregate these segments into one contiuous line per side of the lane.

To accomplish this I apply a primitive algorithm

1. Collect line segments into left and right groups based on their slope and position in the image.
1. Find the maximum and minimum X and Y axis extensions of the entire set of segments. This is the output line.


![Individual segments][images/individual.png]
![Segments coalesced into a line][images/coalesced.png]


### 2. Identify potential shortcomings with your current pipeline

The tracking is highly dependent on contrast. Brightness levels, road material and line wear might decrease contrast enough to make the edge detection fail.

Another problem is obstruction by other vehicles on the road.


Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A great improvement would be to make use of frame-to-frame line persistence, with some kind of exponential fading of  This would improve dashed line tracking.

Another potential improvement could be to ...
