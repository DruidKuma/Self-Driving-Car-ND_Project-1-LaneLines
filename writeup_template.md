# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report
---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps:
1. Convert image to grayscale
2. Apply Gaussian blur
3. Detect edges with the help of Canny Edge Detection algorithm
4. Define region of interest and mask the rest of the image
    * I've defined all points of the polygon of the region depending on image dimensions to be able to run the processing on videos with different resolutions
5. Retrieve Hough lines within the region of interest
6. Overlay lines on the original image

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by adding a sequence of additional steps:
1. Divided all lines on right and left by their slope (and also filtered out noise lines (manually found the best constraints for slopes))
2. For each set of line segments, I averaged and extrapolated the line segments to retrive only one line to draw
    2.1 Divided all x and y points into separate lists
    2.2 Added check in case no satisfactory lines were found
    2.3 Computed coefficients and got line function (y=mx+b) for line extrapolation
    2.4 Computed points for lane line and draw them
    * For left lane I took x1 = 0 and x2 from around top left corner of region of interest (not hard-coded)
    * For right lane I took x1 from around top right corner of region of interest (not hard-coded) and x2 = width of the image
    * All y values were computed by line function mentioned above
    
### 2. Identify potential shortcomings with your current pipeline
One potential shortcoming would be what would happen when a car would ride into a tunnel, or just got onto too dark or too light region on the road.

Another shortcoming could be that if another car would accidentally get into the region of interest, the pipeline would recognize lines on it and this would spoil the result lane lines

### 3. Suggest possible improvements to your pipeline

A possible improvement would be to make grayscale image slightly darker to be able to correctly detect lanes on very light regions of road.

Another potential improvement could be to isolate masks of potential lane colors (e.g. yellow and white) and apply both of them to the image to be able to exclude more noise from the result and to better detect lane lines on too dark or too light regions of the road.