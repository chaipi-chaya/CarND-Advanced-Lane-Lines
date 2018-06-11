## Advanced Lane Finding
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

[//]: # (Image References)

[image1]: ./output_images/undistort_chessboard.png "Undistorted chessboard" 
[image2]: ./output_images/undistort_output.png "Undistorted" 
[image3]: ./output_images/binary_combo_example.png "Binary Example"
[image4]: ./output_images/warped.png "Warp Example"
[image5]: ./output_images/binary_warped.png "Binary Warp Example"
[image6]: ./output_images/left_fit_line.png "Left Fit Visual"
[image7]: ./output_images/right_fit_line.png "Right Fit Visual"
[image8]: ./output_images/left_search.png "Left Search Fit Visual"
[image9]: ./output_images/right_search.png "Right Search Fit Visual"
[image10]: ./output_images/output.jpg "Output"
[video1]: ./project_video_output.mp4 "Video"

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the first code cell of the IPython notebook located in "camera_calibration.ipynb"

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

for other results located in "./camera_cal"

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

All code in pipeline is in "pipeline.ipynb"

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image2]

1. I made a function name "undistort". I extracted values for camera calibration which I saved earlier.

2. use values for undistort function in cv2 for undistort image.

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

1. I made a function call "binarize". I applied 3 gradients which are sobel on lightness of images, saturation and lightness.

2. I combined all 3 gredient with threshold

![alt text][image3]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

1. I made a function name "warp". I defined corner positions for determinating source and destination.

2. I used function "getPerspectiveTransform" for get perspective and inverse of it.

3. I used function "warpPerspective" for wraping images

This resulted in the following source and destination points:


| Source        | Destination   | 
|:-------------:|:-------------:| 
| 258, 682      | 408, 682      | 
| 575, 464      | 408, 0        |
| 707, 464      | 899, 0        |
| 1049, 682     | 899, 682      |

Here are the results.

![alt text][image4]
![alt text][image5]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

1. Before I fit a binary warped image, I marked the image and separated it to left and right.

2. I made 2 functions for fit polynomial name "left_fit_polynomial" and "right_fit_polynomial"

3. This functions take binary image and turn it into histogram and draw line on it.

4. After fit the lines. for next time I use function "left_search_in_margin" and "right_search_in_margin" which take old lines as guideline for find and fit new lines.

Here are the results. I separated it because the result has a problem about fitting the line and unfortunately this isn't help much compare with normal one so I adjusted function "binarize" instead but still kept this functions

![alt text][image6]
![alt text][image7]
![alt text][image8]
![alt text][image9]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

1. I made a function name "get_center_curve" which take lines and images to calculate curvature and vehicle position.

2. I used beginning of each lane to find center of lane then I used center of image as position of the vehicle position and used them to calculate vehicle position respect to center of lane

3. For curvature I use equations in the classroom

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I made a function name "output" for plot all result back down onto the image. And here is the result

![alt text][image10]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video_output.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

I had a problem about choosing the way to make binary images and what threshold to use with it, so I had tried a lot of way.

I still think this binary function is most likely to fail if it got lines that fade or road the have a lot of light.

I think improve binary function could make this more robust.


