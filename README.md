## Advanced Lane Finding
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)


In this project, your goal is to write a software pipeline to identify the lane boundaries in a video, but the main output or product we want you to create is a detailed writeup of the project.  Check out the [writeup template](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) for this project and use it as a starting point for creating your own writeup.  

Creating a great writeup:
---
A great writeup should include the rubric points as well as your description of how you addressed each point.  You should include a detailed description of the code used in each step (with line-number references and code snippets where necessary), and links to other supporting documents or external references.  You should include images in your writeup to demonstrate how your code works with examples.  

All that said, please be concise!  We're not looking for you to write a book here, just a brief description of how you passed each rubric point, and references to the relevant code :). 

You're not required to use markdown for your writeup.  If you use another method please just submit a pdf of your writeup.

The Project
---

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

The images for camera calibration are stored in the folder called `camera_cal`.  The images in `test_images` are for testing your pipeline on single frames.  If you want to extract more test images from the videos, you can simply use an image writing method like `cv2.imwrite()`, i.e., you can read the video in frame by frame as usual, and for frames you want to save for later you can write to an image file.  

To help the reviewer examine your work, please save examples of the output from each stage of your pipeline in the folder called `output_images`, and include a description in your writeup for the project of what each image shows.    The video called `project_video.mp4` is the video your pipeline should work well on.  

The `challenge_video.mp4` video is an extra (and optional) challenge for you if you want to test your pipeline under somewhat trickier conditions.  The `harder_challenge.mp4` video is another optional challenge and is brutal!

If you're feeling ambitious (again, totally optional though), don't stop there!  We encourage you to go out and take video of your own, calibrate your camera and show us how you would implement this project from scratch!

## How to write a README
A well written README file can enhance your project and portfolio.  Develop your abilities to create professional README files by completing [this free course](https://www.udacity.com/course/writing-readmes--ud777).

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

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

---

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

1.) I made a function name "undistort". I extracted values for camera calibration which I saved earlier.

2.) use values for undistort function in cv2 for undistort image.

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

1.) I made a function call "binarize". I applied 3 gradients which are sobel on lightness of images, saturation and lightness.

2.) I combined all 3 gredient with threshold

![alt text][image3]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

1.) I made a function name "warp". I defined corner positions for determinating source and destination.

2.) I used function "getPerspectiveTransform" for get perspective and inverse of it.

3.) I used function "warpPerspective" for wraping images

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

1.) Before I fit a binary warped image, I marked the image and separated it to left and right.

2.) I made 2 functions for fit polynomial name "left_fit_polynomial" and "right_fit_polynomial"

3.) This functions take binary image and turn it into histogram and draw line on it.

4.) After fit the lines. for next time I use function "left_search_in_margin" and "right_search_in_margin" which take old lines as guideline for find and fit new lines.

Here are the results. I separated it because the result has a problem about fitting the line and unfortunately this isn't help much compare with normal one so I adjusted function "binarize" instead but still kept this functions

![alt text][image6]
![alt text][image7]
![alt text][image8]
![alt text][image9]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

1.) I made a function name "get_center_curve" which take lines and images to calculate curvature and vehicle position.

2.) I used beginning of each lane to find center of lane then I used center of image as position of the vehicle position and used them to calculate vehicle position respect to center of lane

3.) For curvature I use equations in the classroom

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


