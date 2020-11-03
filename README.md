## Udacity Advanced Lane Lines Finding Project

### In the previous project of SDCND we explored basic computer vision to perform lane line detection using canny edge detection, ROI and Hough transform techniques to find and draw lane lines on the road. In this project more advanced lane line finding techniques like Gradient, Thresholding, Color transforms and Perspective transform to perform lane finding more robustly.

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

[image1]: ./output_images/undistorted.png "Undistorted"
[image2]: ./output_images/distortion-correction.png "Road Transformed"
[image3]: ./output_images/binary_threshold_combined.png "Binary Example"
[image4]: ./output_images/Perspective_Transform.png "Warp Example"
[image5]: ./output_images/polynomial.png "Fit Visual"
[image6]: ./output_images/plotted.png "Output"
[video1]: ./project_video_output_final.gif "Video output view"
[GIF]: ./project_video_output_final.gif "GIF"




## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points Coverage:

---

### A brief write-up



### Camera Calibration

#### 1. Computation of camera matrix and distortion coefficients & distortion correction

The code for this step is contained in the second code cell of the IPython notebook located in "./CarND-Advanced-Lane-Lines_Solution.ipynb"   

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]

### Pipeline (single images)

#### 1. Example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to test images.
![alt text][image2]

#### 2. Example of a binary image result.

Used a combination of color and gradient thresholds to generate a binary image.

![alt text][image3]

#### 3. Example of a transformed image.

The code for my perspective transform includes a function called `warp_image()`, which appears in the 6th code cell of the IPython notebook.  I chose the hardcode the source and destination points in the following manner:

```python
    src = np.float32(
        [
            [580.0, 460.0],
            [740.0, 460.0],
            [1100.0, 670.0],
            [270.0, 670.0],
        ]
    )

# offset_value = 200.0

    dst = np.float32(
        [
            [offset_value, 0],
            [width - offset_value, 0],
            [width - offset_value, height],
            [offset_value, height],
        ]
    )
```

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 580, 460      | 200, 0        | 
| 740, 460      | 1080, 0      |
| 1100, 670     | 1080, 720      |
| 270, 670      | 200, 720        |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image4]

#### 4. Identification of lane-line pixels and fitting with a polynomial.

Used histogram of the binary image to find the left lane base starting point, right lane base starting point

Implemented sliding window method to follow the respective lane line pixels.

Used them to fit a 2nd order polynomial to project the lane line.

![alt text][image5]

#### 5. Calculation of the radius of curvature of the lane and the position of the vehicle with respect to center.

These steps are implemented in the `code cell 18`

#### 6. Example image of result plotted on the road.

In the given image both the lane line is plotted clearly, and the radius of curvature, Car position from the center is displayed.

![alt text][image6]

---

### Pipeline (video)

#### Final video output given in a GIF format for convenience.

Here's a glimpse of the output

![alt text][GIF]

---

### Discussion

#### Room for improvement.

Overall, this solution is advanced than the previous project of finding lane lines, eventhough many new techniques are used to get the performance that is required there would be still some limitations due to various weather and light conditions at the given environment.

I think point of developing a single solution, which could work for all the given conditions must also involve premature step such as analysing the surrounding environment of the vehicle is in and perform optimization in the lane line finding solution to be implemented accordingly. so that it works well for all.

---

Reference:

1. [Advanced Lane Finding](https://medium.com/typeiqs/advanced-lane-finding-c3c8305f074)

2. [Teaching Cars To See — Advanced Lane Detection Using Computer Vision](https://towardsdatascience.com/teaching-cars-to-see-advanced-lane-detection-using-computer-vision-87a01de0424f)

---
