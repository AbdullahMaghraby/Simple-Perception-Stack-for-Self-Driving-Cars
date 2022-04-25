# Simple-Perception-Stack-for-Self-Driving-Cars
In this project we are going to create a simple perception stack for self-driving cars (SDCs.) Although a typical perception stack for a self-driving car may contain different data sources from different sensors (ex.: cameras, lidar, radar, etc…), we’re only going to be focusing on video streams from cameras for simplicity. We’re mainly going to be analyzing the road ahead, detecting the lane lines, detecting other cars/agents on the road, and estimating some useful information that may help other SDCs stacks. The project is split into two phases. We’ll be going into each of them in the following parts.

## Pipeline architecture
1-Compute Camera Calibration.
2-Apply Distortion Correction.
3-Apply a Perspective Transform.
4-Create a Thresholded Binary Image.
5-Define the Image Processing Pipeline.
6-Detect Lane Lines.
7-Determine the Curvature of the Lane and Vehicle Position.
8-Visual display of the Lane Boundaries and Numerical Estimation of Lane Curvature and Vehicle Position.
9-Process Project Videos.


## Camera Calibration
we need to calibrate camera as our test video have curved lanes so to use Distortion Correction we need to know camera parameters

## Apply Distortion Correction


## Apply a Perspective Transform
convert the vehicle’s camera view of the scene into a top-down “bird’s-eye” view

## Create a Thresholded Binary Image
using color space for lane detection

### LAB Color Space
The Lab color space describes mathematically all perceivable colors in the three dimensions L for lightness and a and b for the color opponents green–red and blue–yellow.

### HLS color space
This model was developed to specify the values of hue, lightness, and saturation of a color in each channel. The difference with respect to the HSV color model is that the lightness of a pure color defined by HLS is equal to the lightness of a medium gray, while the brightness of a pure color defined by HSV is equal to the brightness of white.

## Color Space Thresholding
the white lane lines are clearly highlighted in the L-channel of the of the HLS color space, and the yellow line are clear in the B-channel of the LAP color space as well. We'll apply HLS L-threshold and LAB B-threshold to the image to highlight the lane lines.

## Image Processing Pipeline
Distortion Correction then Perspective Transform thenColor Thresholding

## Detect the Lane Lines
we still need to decide explicitly which pixels are part of the lines and which belong to the left line and which belong to the right line

### Sliding Window Search
We'll compute a histogram of the bottom half of the image and find the base of the left and right lane lines. Originally these locations were identified from the local maxima of the left and right halves of the histogram, but in the final implementation we used quarters of the histogram just left and right of the midpoint. This helped to reject lines from adjacent lanes. The function identifies 50 windows from which to identify lane pixels, each one centered on the midpoint of the pixels from the window below. This effectively "follows" the lane lines up to the top of the binary image, and speeds processing by only searching for activated pixels over a small portion of the image

## Polyfit Using Fit from Previous Frame
The Polyfit Using Fit from Previous Frame is another way that performs basically the same task, but alleviates much difficulty of the search process by leveraging a previous fit (from a previous video frame, for example) and only searching for lane pixels within a certain range of that fit.

## Visual display of the Lane Boundaries


## Lane Curvature and Vehicle Position from center of the lane
The curvature was calculated ussing this line of code:

curve_radius = ((1 + (2*fit[0]*y_0*y_meters_per_pixel + fit[1])**2)**1.5) / np.absolute(2*fit[0])

In this example, fit[0] is the first coefficient (the y-squared coefficient) of the second order polynomial fit, and fit[1] is the second coefficient. y_0 is the y position within the image upon which the curvature calculation is based. y_meters_per_pixel is the factor used for converting from pixels to meters. This conversion was also used to generate a new fit with coefficients in terms of meters.

The position of the vehicle with respect to the center of the lane is calculated with the following lines of code:

lane_center_position = (r_fit_x_int + l_fit_x_int) /2 center_dist = (car_position - lane_center_position) * x_meters_per_pix

r_fit_x_int and l_fit_x_int are the x-intercepts of the right and left fits, respectively. This requires evaluating the fit at the maximum y value because the minimum y value is actually at the top. The car position is the difference between these intercept points and the image midpoint (assuming that the camera is mounted at the center of the vehicle)


## Writting Vheicle Position and Lane Curvature

## Process Project Videos


# Hint all Pictures and Videos for tests are provided in Jupyter Notebook



