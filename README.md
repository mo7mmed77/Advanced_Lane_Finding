## Advanced Lane Finding Project

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

[image1]: ./output_images/Distortion_Correction.png "Undistorted"
[image2]: ./output_images/Undistorted_Raw_Image.png "Undistorted"
[image3]: ./output_images/Combined_Transform.png "Road Transformed"
[image4]: ./output_images/Perspective_Transform_Ex.png "Perspective Example"
[image5]: ./examples/warped_straight_lines.jpg "Warp Example"
[image6]: ./examples/color_fit_lines.jpg "Fit Visual"
[image7]: ./examples/example_output.jpg "Output"
[video1]: ./project_video.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

### Camera Calibration

The code for this step is contained in the first code cell of the IPython notebook located in ".My_pipeline.ipynb". As well as an example of the output of this step on the chess image.  

I did that by starting to prepare "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image1]


#### Distortion Correction on raw image

To demonstrate this step on a raw image, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image2]

A function (cal_undist), in the third cell,  is created to take the image, objpoints and imgpoints (from the previous step) and return an undistorted version of the given image. 

#### Color transforms and gradients methods to create a thresholded binary image

I used a combination of color and gradient thresholds to generate a binary image (color thresholding steps at fifth cell, while the gradient thresholding are done in the sixth cell). I used HLS with threshholds on the saturation between 130 and 255, while three thresholds for the gradients ( sobel x, sobel magnitude and sobel direction). .  Here's an example of my output for this step.

![alt text][image3]

#### Perspective transform.

The code for my perspective transform includes a function called `Persp_trans()`, which appears in the forth cell of the My_pipeline.  This function takes as inputs an image (`img`), and returned a transformed image (top-down view of the road).  I chose the hardcode the source and destination points in the following manner:

```python
    src=np.float32([ (731,455),(570,455),(1300,690),(120,690) ])

dst = np.float32([[img_size[0], 0], [0, 0], 
                                     [img_size[0], img_size[1]], 
                                     [0, img_size[1]]])
```

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 731, 455      | 720, 0        | 
| 570, 455      | 0, 0          |
| 1300, 690     | 1280, 720     |
| 120, 690      | 0, 720        |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![alt text][image4]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

Then I did some other stuff and fit my lane lines with a 2nd order polynomial kinda like this:

![alt text][image5]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in lines # through # in my code in `my_other_file.py`

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in lines # through # in my code in `yet_another_file.py` in the function `map_lane()`.  Here is an example of my result on a test image:

![alt text][image6]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./project_video.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Here I'll talk about the approach I took, what techniques I used, what worked and why, where the pipeline might fail and how I might improve it if I were going to pursue this project further.  
