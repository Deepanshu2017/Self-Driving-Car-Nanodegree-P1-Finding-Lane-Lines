# **Finding Lane Lines on the Road** 

<!-- ---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report -->


[//]: # (Image References)

[image1]: ./first.png "Original"
[image2]: ./gray.png "GrayScale"
[image3]: ./blur.png "Gaussian Blur"
[image4]: ./canny.png "Canny Edges"
[image5]: ./roi.png "ROI"
[image6]: ./hough.png "Hough"
[image7]: ./output.png "Output"

---

### Reflection

In this project we need to find and identify the lanes on the road and create a 
modified line on the identified lane.

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My Pipeline consist of 5 major steps. That are -
 - Converting Images to GrayScale
 - Gaussian Blur
 - Apply Canny Edge detector on blurred image
 - Identifying and selecting Region of interest
 - Getting lines of interest using Hough
 - Merging images using `cv2.addWeighted`

Below is the explanation of each of the steps

Original Image looks like this:
![alt text][image1]

First step is necessary to convert our 3 channel image to single channel image. 
Output of GrayScale image is as below.
![alt text][image2]

After converting images to GrayScale, next step in my pipeline is the Gaussian Blur. 
Gaussian Blur is applied to reduce noise in the images and this smooths the image.
For Gaussian Blue I used kernel of size 7.
![alt text][image3]

After applying the Blur next step is to apply the Canny Edge Detector which detectes
the edges using gradients. The low threshold for canny function is 50 and high 
threshold for canny function is 150. I tried various values and 50, 150 seems to work
best.
![alt text][image4]

As we can see the canny edge detector has detected all the edges but not all of them
are of our interest. So we need to define the region of interest, i.e. the region
in which we are most interested. In this case our lanes. For the ROI, I created
vertices and choosed parameters with hard coded coordinates.
![alt text][image5]

Now we need to connect these disconnected lines, in this hough transform helps us.
The Hough transform, internally calls the draw function which is one of the things
function I had to do most of the logic. The logic that I came up with is get the 
slope of the line, if lines slope is negative then line is a left line, if slope is
position then line is right line (right lane). Then for all the left lines and right
lines, I used `np.nanmean` to get the average coordinate of `x` for left and
right. After getting them I plotted the lines from these coordinates to `min` and
`max` of `y`.
![alt text][image6]

After getting the images, the only thing left was to combine the output with original
image.
![alt text][image7]

### 2. Identify potential shortcomings with your current pipeline

As par me, one of the biggest short coming is in the region of interest mask with
assumption in draw that lines are straight. The hard coded part can be and should be dynamic. 
The mask might stop to work properly if lines are not straight or car is
not in the center. The draw function takes the lines and always assumes there 
are two lanes. Also it's assumption that the lines are straight.


### 3. Suggest possible improvements to your pipeline

As long as the car drives precisely in the middle, the roi mask with draw work
perfectly but dynamic change in the mask might help improving the filteration
process and draw should be find the curved lines. This is why first two videos
are working perfectly for me but third one is failing. So working on this is on my
top priority. One possible solution is to keeping the mean of roi mask and changing
it dynamically according to the car movement.