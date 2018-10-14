**Finding Lane Lines on the Road**

**Steven Moffatt - October 14, 2018**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/P1_solidWhiteCurve.jpg
[image2]: ./test_images_output/P1_solidWhiteRight.jpg
[image3]: ./test_images_output/P1_solidYellowCurve.jpg
[image4]: ./test_images_output/P1_solidYellowCurve2.jpg
[image5]: ./test_images_output/P1_solidYellowLeft.jpg
[image6]: ./test_images_output/P1_whiteCarLaneSwitch.jpg

[video1]: ./test_videos_output/P1_solidWhiteRight.mp4
[video2]: ./test_videos_output/P1_solidYellowLeft.mp4


---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps:

1). Converted the image to grayscale & applied gaussian blur. 

2). Extracted the edges to lines using Canny detection. 

3). Extracted those edges in the region of interest (polygon).

4). Performed hough processing on the remaining edges to identify lane lines.  

5). Displayed the lane lines overlayed on the base image with weighting function.  


In order to draw a single line on the left and right lanes, I modified the draw_lines() to:

1). calculated lines slope and y-intercept. 

2). Seperated positive and negative slope lines into different processes. 

3). Averaged the slopes and intercepts for both left and right. 

4). Ran edge case processing to cancel processing of 1). no line found or 2). vertical line found.

5). Generated the overlayed line by using the limit y-values of the region of interest. 

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


Potential shortcomings include (would be what would happen when): 

1). When a third or additional lane lines would be identified by the filter (bike lanes).  The draw_lines function assume 0-2 lane lines.  

2). The averaging mechanism I built into draw_lines means if debris or lane markings are mis-classified and there aren't many line segments identified, there would be a lot of noise (variability/ jitter) in the lane classification. 

3). Corners or turns that require identification outside of the polygonal extraction area would be problematic.  The pipeline assumes straight lane lines.  

4). In my code for P1_solidYellowLeft.mp4 there is minor flickering on the right lane.  I'd like to know what is causing this.  


### 3. Suggest possible improvements to your pipeline

Possible improvements include:

1). It's super awkward to process hough lines in polar for reasons of infinite slope and then build the pipeline using helper functions that process in cartesian.  This made it so all the special cases of division by zero and NaN float errors had to be handled. I would like to extract the averaged line from the hough function after checking if it is within a certain tolerance.  I think this is the bulletproof way of doing straight lines. 

2). Use the above to build out lines in addition to 0, 1, and 2 line solutions.  You can do a node check and then a count like function to know what you're seeing.  Compare to established lane norms (symmetery) for knowing if something is a line or if it is a car in front of you.  

3). Build in polynominal line modelling vs straight lines.

4). Dive deeper in hough classification on P1_solidYellowLeft.mp4 to find out why the right lane is obvious, but periodically it breaks down.  

