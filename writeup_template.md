# **Finding Lane Lines on the Road** 

[//]: # (Image References)
[image1]: ./examples/grayscale.jpg "Grayscale"
[image2]: ./examples/line-segments-example.jpg "Hough Transformation"
[image3]: ./examples/laneLines_thirdPass.jpg "Final Lane lines"

---
### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

To find the lane lines on a road, a pipeline consisting of computer vision techniques is implemented in following sequence:
* Input image is converted into grayscale image.
![alt text][image1]
* A gaussian blur is applied to the image to decrease noise.
* Canny edge detection is used to isolate prominent edges in the image.
* Region marking is applied to focus on relevant part of the image
* Hough Tranformation is used to convert edges from pixel form to analytical line form.
![alt text][image2]
* Postprocessing is done to calculate final left and right lane lines.
* Initial and final images are superimposed to create annotated output.
![alt text][image3]

To draw a single line on the left and right lanes, the the draw_lines() function was modified. All the lines recieved using Hough transformation are divided into two groups using slope as a metric. Lines with positive slope is assumed to be a part of left lane while lines with negative slope comprises the right lane. Lines with relatively less magnitude of slope (-.25<m<.25) don't represent any of the lanes and are not considered. Finally, the unique line for each lane is described using a point and its slope.  Point is calculated by averaging all the end points of constituent lines while the final slope is taken as median of the constituent slopes.


### 2. Identify potential shortcomings with your current pipeline

Some of the limitations of the above described approach are:
* Inaccuracies due to changes in road material from darker asphalt to concrete (like on bridges or overpasses).
* Inaccuracies due to shadows from nearby objects on the road.
* Inaccuracies due to Ggrbage in lanes, especially near dash lines. 


### 3. Suggest possible improvements to your pipeline

* One possible improvement could be using data from previous timestep to calculate lane readings. It is obvious that drastic changes in lane location is impossible within negligible time and smoothening the changes would benefit the pipeline.
* Use of lane colors to isolate the lanes could also be better than using a grayscale image.
* Use of curves instead of straight lines could be used to better represent the lane lines
