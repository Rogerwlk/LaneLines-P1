# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[origin]: ./writeup_images/origin.png

[color_mask]: ./writeup_images/color_mask.png

[grayscale]: ./writeup_images/grayscale.png

[smooth]: ./writeup_images/smooth.png

[edge]: ./writeup_images/edge.png

[region_mask]: ./writeup_images/region_mask.png

[two_lines]: ./writeup_images/two_lines.png

[result]: ./writeup_images/result.png

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 7 steps.

Original image:

![original image][origin]

1. Create a color mask that only keeps the color yellow and white from the original image. The upper bound is (255, 255, 255) white. The lower bound is (210, 160, 0). I found the yellow sign color's rgb value (255, 204, 0) from this website http://www.trafficsign.us/signcolor.html.

![color mask image][color_mask]

2. Convert the image to grey scale.

![grayscale image][grayscale]

3. Apply Gaussian smoothing with a kernel size 9. The image is slightly blurred.

![smoothed image][smooth]

4. Canny edge detection with low_threshold of 70 and high_threshold of 180.

![edge detected image][edge]

5. Apply a region mask that cut out a trapezoid region in the image.

![region mask image][region_mask]

6. Use Hough to identify small line segments to draw two long boundary lines on the left and right side.

![two lines image][two_lines]

7. Add the two long boundary lines on top of the original image.

![result image][result]

draw_lines() function details:

In order to draw a single line on the left and right lanes from small line segments. I modified the draw_lines() function. I first split the line segments into left part and right part according to line's x1 value. Then, on each part, I run a RANSAC regressor that try to fit a line covering as many points as possible. It is very robust. This regressor will output image's x coordinate given image's y coordinate. Given the fitted line, I can predict the line's two x coordinates given a bottom y value and a y value close to the center. At last, I draw two lines from the predicted coordinates.

challenge details:

I changed all the hard-coded coordinate value to a percentage of the image size.

I added a color mask to filter out distracting color such as dark tree shadow and light colored road.

I made a program to capture the screenshots from the video. The program was not able to run in the workspace. I ran it locally and uploaded some difficult screenshots for testing. The screenshot program is called "video_frame.ipynb".


### 2. Identify potential shortcomings with your current pipeline

1. If the light is too bright/dark. The color mask may include all/no region. Then the edge detection may not identify any edges because colors are very similar in the bright/dark environment.

2. If the line color is not white or yellow, the color mask may filter out side lines.

3. If the camera is not positioned correctly, the region mask may filter out side lines. The draw_lines() function can't identify two good lines.

4. If there are some distracting white or yellow paintings on the road, the RANSAC will misidentify the most fitted lines.


### 3. Suggest possible improvements to your pipeline

1. Limit the slope of the most fitted line to a certain range so that it won't be close to horizontal or vertical.

2. Optimize the pipeline to improve speed performance. It can be reducing the number of steps, using faster method to process numpy array etc.

More possible improvements suggested by reviews:

3. Image from infrared camera.

4. Adding a outlier reduction approach like RANSAC on the hough lines. (already used)

5. Using curve fitting to plot the curve instead of straight lines.

[RANSAC.pdf](https://udacity-reviews-uploads.s3.us-west-2.amazonaws.com/_attachments/50281/1544280056/RANSAC.pdf)

[curve.pdf](https://udacity-reviews-uploads.s3.us-west-2.amazonaws.com/_attachments/50281/1544280080/curve.pdf)