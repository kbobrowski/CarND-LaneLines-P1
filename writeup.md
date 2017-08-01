# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[test_frame]: ./writeup_images/test_frame.PNG "Test frame"
[colors]: ./writeup_images/colors.PNG "Colors selection"
[edges]: ./writeup_images/edges.PNG "Edges"
[hough]: ./writeup_images/hough.PNG "Hough lines"
[extrapolated]: ./writeup_images/extrapolated.PNG "Extrapolated lines"
[hough2]: ./writeup_images/hough_lines.png "Hough lines2"
[hough2_groups]: ./writeup_images/hough_lines_groups.png "Clustered Hough lines"
[stabilized]: ./writeup_images/stabilized.PNG "Stabilization"

---

### Reflection

### 1. Description of the pipeline

My pipeline consisted of 5 steps. First, I extracted yellow and white colors, then I detected edges using Canny algorithm. In the next step I masked the edges using polygon, followed by detection of lines using Hough algorithm. Finally I extracted left and right lines from a set of detected lines. These steps are visualized using test frame from the challenge video:

Test frame:

![alt text][test_frame]

Color selection:

![alt text][colors]

Extracted edges in the region of interest:

![alt text][edges]

Hough lines:

![alt text][hough]

Extrapolated Hough lines:

![alt text][extrapolated]

In order to extract left and right lines from a set of Hough lines, I modified draw_lines to perform following steps:
- calculation of slope (a), y-intercept (b) and length of each line (described as y=ax+b)
- normalizing of a,b space with StandardScaler from sklearn
- detecting clusters in a,b space (lines closely co-linear) using DBSCAN method with lines lengths as weights
- calculation of the centers of the clusters using weighted average with line lenghts as weights
- selection of two largest clusters for which the slopes have opposite signs and absolute value in a range (0.2, 1.5)
- transforming a,b of selected two clusters back to original scale
- optionally for the purpose of the movie: stabilizing slope and y-intercept of the lines by averaging last 20 frames
- drawing lines with lower bound as a bottom of the frame and upper bound defined by ylim variable passed to draw_lines

Robustness of left and right line selection is visualized in the following example (DBSCAN identifies 3 clusters, and weighted centers of two most significant are taken as final left and right lines, one accidental line is discarded):

![alt text][hough2]
![alt text][hough2_groups]

Finally, line stablilzation is visualized in the following plots (corresponding to the challenge video):

![alt text][stabilized]


### 2. Identify potential shortcomings with your current pipeline

Potential shortcoming might include:

- numerical errors in Hough lines clustering if lines is closely vertical (calculation of slope would include division by zero / very small number)
- situation where lines does not have opposite slopes is not handled
- no tracking of turns - lanes are always assumed to be straigh

### 3. Suggest possible improvements to your pipeline

Possible improvements include:

- Cluster Hough lines in coordinate system which would avoid division by zero / very small number
- Allow for slopes of the lines with the same sign (e.g. when the car is joining a new lane), although it would require a special treatment of double lines
- Make lane model more complex (e.g. described by non-linear equation), to allow accurate lane tracking in turns
