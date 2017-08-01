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

### 1. Description of the pipeline

My pipeline consisted of 5 steps. First, I extracted yellow and white colors, then I detected edges using Canny algorithm. In the next step I masked the edges using polygon, followed by detection of lines using Hough algorithm. Finally I extracted left and right lines from a set of detected lines.

In order to extract left and right lines from a set of Hough lines, I modified draw_lines to perform following steps:
- calculation of slope (a), shift (b) and length of each line (described as y=ax+b)
- normalizing of a,b space with StandardScaler from sklearn
- detecting clusters in a,b space (lines closely co-linear) using DBSCAN method with lines lengths as weights
- calculation of the centers of the clusters using weighted average with line lenghts as weights
- selection of two largest clusters for which the slopes have opposite signs and absolute value in a range (0.2, 1.5)
- transforming a,b of selected two clusters back to original scale
- optionally for the purpose of the movie: stabilizing slope and shift of the lines by averaging last 20 frames
- drawing lines with lower bound as a bottom of the frame and upper bound defined ylim variable passed to draw_lines

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]

<video width="960" height="540" controls>
  <source src="test_videos_output/solidWhiteRight.mp4">
</video>


### 2. Identify potential shortcomings with your current pipeline

- line vertical
- situation where lines does not have opposite slopes
- no tracking of turns
- 

One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

- allow broader range of shape for the line
- 

A possible improvement would be to ...

Another potential improvement could be to ...
