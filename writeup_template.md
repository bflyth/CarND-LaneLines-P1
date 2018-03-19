# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps. First, the image is loaded and converted to grayscale. A gauss filter is applied to the image. I chose a Kernel of 5 after trying 1, 3, 5, and 7 and seeing little to no difference. The gauss filtered image is then sent to the canny filter with a low threshold of 60 and a high threshold of 180. This satisifies the recommended 1:2 or 1:3 ratio. After trying aproximately 70 to 140, 60 to 180 appeared to make lines a little crisper, and really defined the differences between road and line. With a canny filtered image, a region is applied to extend approximately 4 car lengths away (with the current camera perspective) and only extends 6-12 inches from the oustsides of each lane side. Within the regionally filtered space, a hough filter is applied with a rho of 1, mininum line length of 5, max line gap of 5, and an intersectional threshold of 5. After drawing the hough lines, the original image is filtered under the lines with weighted_img.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by including 4 functions. Slope, insert, filters, and reduce_avg. Slope() is simple, and determines the rise and run for two points, and then determines the slope. This was already included in draw_lines. Filters() uses standard deviation to filter the slope. Insert() interpolates the slope, while reduce_avg returns the values into a set, which then is used to find the floor and ceiling of each x and y value to draw the line.


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when using the draw lines function. The functions within draw_lines() are vague and I saw that in some images the line was not quite on the lane line. This can also be seen in the video when the car shakes on a bump or slight turn, the lines also shake around sometimes deviating greatly.

Another shortcoming could be using this pipeline on a curved road. The challenge video is a righthand curve, but the way the pipeline is set up leaves a regional filter facing strictly forward. This forward region would never cover either a left or right hand curve or turn.

### 3. Suggest possible improvements to your pipeline

A possible improvement would be to refine the draw_lines function to run a more accurate filter, but this may be in part with image quality and canny/hough resolution. 

Another potential improvement could be to add some method that could understand the direction a yellow line is headed (or for other lanes on the road: the dotted white line) and correct the region to follow this line.
