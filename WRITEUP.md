## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

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

[image1]: ./submission_imgs/undistortion.JPG "Undistorted"
[image11]: ./submission_imgs/undistortion_example.JPG "Undistorted example"
[image2]: ./submission_imgs/Sobel_direction.JPG "Sobel Direction"
[image3]: ./submission_imgs/grad_magnitude.JPG "Gradient Magnitude"
[image4]: ./submission_imgs/grad_direction.JPG "Gradient Direction"
[image5]: ./submission_imgs/combined.JPG "Combined"
[image6]: ./submission_imgs/transformed.JPG "Transformed"
[image7]: ./submission_imgs/fit_windws.JPG "Windows_fitted"
[image8]: ./submission_imgs/final.JPG "Final output"
[video1]: ./project_video.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

To calculate the camera matrix and distortion coefficients I used the function cv2.calibrateCamera().
With the 'findChessboardCorners()' function I calculated the image points. 
I applied the distortion correction to the test image using the 'cv2.undistort()' function and obtained this result:

![alt text][image1]


### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

An undistorted image of the road:

![alt text][image11]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image. To determine which is the best combination I tried different methods.
Finnaly I decided a combination of Gradient Magnitude and Color thresholding provides the best result. The cmbination of the results happens in the 'apply_gradmag_color_thresh()' function.
A overview of the different methods on the same image:

![alt text][image2]
![alt text][image3]
![alt text][image4]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.


In coded an method to choose src points by mouse click. When running the code the image will open and you can choose 4 points and exit the window by pressing `x`.
Based on the selected src points I calculated the dst points.
I verified that my perspective transform was working as expected by looking at the lines. They appear parallel in the warped image, so the transformation has to be more or less accurate.


![alt text][image6]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

In the function fit_windows(), which takes as Input a black and white warped image, I calculated the histogram of the image. That gave me information about the beginning of the lanes. From there I used the provided code to fit windows to get the x and y position of the lane pixels.
Now it is possible to fit a second order polynom, one for the right and one for the left lane. The return value of the 'fit_windows()' function are the polynomial coefficients.

![alt text][image7]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

In the function  'calc_radius_dist()' I calculated the radius of the curvature and the distance from the lane center.
I used the conversions in x and y from pixels space to meters which were provided. To get the mean curvature I added the left and the right curvature and divided the sum by 2.

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

![alt text][image8]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  
