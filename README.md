# **Finding Lane Lines on the Road** 

When humans drive on the road, we rely heavily on lane line markings to steer our vehicles. The objective of this project is impart this ability of identifing lane lines to a computer. 

Images and video files from a dashcam mounted in a car is used as input. Jupyter notebook, Python and OpenCV is used to detect and visualize the lane lines in input. The anaconda environment being used is available as the [CarND Term1 Starter Kit](https://github.com/udacity/CarND-Term1-Starter-Kit/blob/master/README.md).


## Image Processing Pipeline:

To find the lane lines on a road, a pipeline consisting of computer vision techniques is implemented in following sequence:
1. **Input:** An Input image is received by the pipeline. It can be scaled down to improve algorithm speed.
2. **Grayscaling:** First operation is the grayscaling operation. The operation reduces the color channels from three to one. This improves algorithm speed. 
3. **Blurring:** Then, a gaussian blur is applied to decrease noise. A `kernel_size= 5x5` was emperically determined for this project.
4. **Edge Detection:** Next, Canny edge detection is used to isolate prominent edges in the image. A `low_threshold=50` and `high_threshold=150` for pixel intensity were emperically determined for this project. The result is a binary image. 
5. **Region Masking:** The relavant part of the image which contain lane information is then isolated using a polygon mask. This eliminates the extraneous edges from the images.
6. **Hough Transform:** To convert pixel data into raw line segment data, Hough transform is used. The hyper-papameters are emperically determined. Resolution parameters are `rho=2px` and `theta=2 degrees`. Minimum intersection threshold was set as `threshold=40`. The line parameters were set as `min_length=30px` and `max_gap=20px`.
7. **Slope Thresholding:** Raw line segment data is classified into left and right lanes depending on slope values. Line segments whose slopes satisfy `-.25 <slope <.25` are not considered since these lines don't represent lane lines.
8. **Line Averaging:** Raw lane lines are aggregated into unique lane line hence completing the lane detection process. Aggregation is done using average of slopes and midpoints of raw line segments.
9. **Final Bisualization:** The detected lane lines are superimposed with initial dashcam image for visualization and analysisng errors.

The figure below displays each step in the pipeline.

<img src="test_images_output/solidWhiteCurve_detail.jpg" width="900" title="Image Processing Pipeline"/>




## Limitations
A huge hurdle in generalizing this approach is the selection of optimum Image Processing hyper-parameters. Since these parameters were determined emperically using very limited dataset (single car, one highway, daytime, sunny, etc.), the algorithm works poorly if used in other situations. Sample situations where above described approach could work poorly are:
* Changes in road material from darker asphalt to concrete (like on bridges or overpasses).
* Shadows on the road from nearby objects (like trees or buildings).
* Debris/dirt in lanes, especially near dash lines. 
* Change in weather conditions (like rainy or snowy).
* Varied lighting at different times in a day or night.


## Possible Improvements

* One possible improvement could be using data from previous timestep to calculate lane readings. It is obvious that drastic changes in lane location is impossible within negligible time and smoothening the changes would benefit the pipeline.
* Use of lane colors to isolate the lanes could also be better than using a grayscale image. However, that would require more computation power.
* Use of curves instead of straight lines could be used to better represent the lane lines
