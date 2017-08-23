# Advanced Lane Finding - P4

### Overview
The aim of this project was to return to image processing and computer vision techniques to create a more robust lane detection pipeline. My pipeline consists of breaking an image out into it's 3 color channels, using a combination of thresholding and gradient detection techniques, warping the image's perspective and using some simple pixel histogram/windowing to detect the lane lines. Once both the left and right lane lines are found, the pipeline is essentially reversed to invert the image transform back to the original camera's perspective, thereby mapping and highlighting the found lane line back onto the road from the driver's perspective.

#### 1 | Camera Calibration
The camera was calibrated using the twenty 9x6 chessboard test images. Using the OpenCV (CV2) framework, the test images were fed into the native `findChessboardCorners` function and the `calibrateCamera` function returned the calibration matrices and coefficients.

An example image road test image is present below, after applying the `undistort` function with the aforementioned calibration data.

[original_undist]:./undistorted.png
![Original image and Undistorted][original_undist]

#### 2 | Perspective Transforms
I did a perspective transform prior to producing the binary thresholded images. I selected a four-sided polygon starting from the bottom left corner and followed it up clockwise (CW) to about midway in the current lane. An example of a perspective transformed image is given below:

[warped]:./warped.png
![Original and Warped][warped]

#### 3 | Gradients and Binary Thresholding 
I experimented with some simple Sobel, color channel and magnitude thresholding. My resulting binary thresholded image used a mixture of color thresholding, grabbing yellow from the HSV image, white from HSV, white from HLS and white from the RGB image. An example result would be:

[threshold_example]:./threshold.png
![Binary Thresholded Result][threshold_example]

#### 4 | Lane Finding
I used a simple histogram method to find the lane lines. This method simply took windowed sliced from the thresholded image and looked for an accumulation of white pixels and pieced them together. For example, a histogram for the binary image above  would look like this:

[histo]:./histo.png
![Binary Thresholded Result][histo]

I used a 72-window, and values of 50 and 20 for margin and min pixels. Subsequent processing to fit a second-degree polynomial and drawing our found left and right lane lines could look like the following:

[histo2]:./histo2.png
![Binary Thresholded Result][histo2]

[histo3]:./histo3.png
![Binary Thresholded Result][histo3]

#### 5 | Radius of Curvature
The radii were calculated given the example code and pixel-to-meter conversion factors. Additionally, the vehicle's position from center was taken by finding the midpoint of the x-intercepts of the found left and right lanes. The video updates the radii and vehicle position every few frames.

#### 6 | Finished Pipeline
The pipeline works pretty well against the simple project video. An example output is given below, showing the highlighted lane:

[pipeline_example]:./pipeline_output.png
![Example Output from Pipeline][pipeline_example]

A snippet (25s) of my processed `project_video` is shown below (GIF):

[final]:./project_video_output.gif
![Example Output from Pipeline][final]

The [Full MP4 video](https://github.com/ydodia/SDCND-Term1-P4/blob/master/project_video_output.mp4) is linked from my GitHub.

#### 7 | Challenge Video
Using the yellow and white color thresholding, I was able to extract the lane lines better and successfully identify lanes in the challenge video:


[challenge]:./challenge_video_output.gif
![Example Output from Pipeline][challenge]

The [challenge video](https://github.com/ydodia/SDCND-Term1-P4/blob/master/challenge_video_output.mp4) is linked from my GitHub.

### Conclusion
I ran into several difficulties; in particular, with image pre-processing, windowing prior to the perspective transform, finding the lane lines and finally, dealing with poor or outlier lane lines.

Image pre-processing is critical and I'd like to have spent a bit more time on that. I was able to get clear and low-noise outputs mostly by using color channel (S and R) thresholding.

Choice of windowing, or selecting the four-sided polygon, for the perspective transform greatly controls the amount of data you let into your pipeline. It might help to create a 'longer' window following the road further to better find lane lines.

Outlier lines were ones that were typically noisy and lacked good data points. I thresholded these out of each Line's data and used averaging to maintain a smoother fit.

In the future, I would focus on three areas: better image pre-processing, robust statistical analysis of lane markers and a better Line class adding look-ahead and smarted line following.






