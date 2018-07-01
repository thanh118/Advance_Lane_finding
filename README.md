## Advanced Lane Finding

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

CAMERA CALIBRATION AND DISTORTION CORRECTION

The transform of 3D object in the real world to a 2D image is'nt perfect. We have to correct the image distortion because it changes the apprance size, or shape of an object and more importantly it make object appear closer or father away than they atually are.

A chessboard can be use because its regular high contrast pattern makes it easy to detect and measure distortions as we know how and undistorted chessboard look likes. If we have multiple pictures of same chessboard again a flat surface from camera, we can get the from the different between a apparent size and shape of the images compared to what it should theoreticle be. We can create a transform that map the distorted points to undistorted point, and use this transform to undistort image.


I have made a chessboard class specifically for this. It tales a path to a chessboard iamges, and the number of chessborad corners in each rows and collumns it expects to detect(in this case  9 and 6 respectively). It use the opencv functions find chessboardCorner() and drawchessboardCorners(). For each of the twenty chessbaord images provided in camera_cal folder, We can get the corner(if possible), and give a set of points(object points) that map the corner points coordinates to the theoreticle corrdinate ( for example (0, 0, 0), _(0, 1, 0), (8,5,0)). So we can get the camera calibration paremeter( distrotions_coefficients, camera_calibration_matrix) that we can use to undistort an image. We use a build-in calibrateCamera() funtion of open cv for this. We feed this parameters to build in openCV undistort() funtion to get the corrected image.

GETTING THE “SKY VIEW” WITH PERSPECTIVE_TRANSFORM 

Given an undistorted image of the vehilce perspective(vehicle view), we can wrap this image to output an image of another perspective such as a bird's eye view from the sky(sky_view) given a trasformation matrix. We can derive transformation matrix( wrap matrix) by giving the pixel corrdinates of points of the input image of one perspective( source_points) and the corresponding pixels coordinates of the output perspective (destination points) using the function getPerspectivetransform().We also can get the inverse_warp_matrix to get from "sky_view" to "vehicle_view" by switching the places of the source and destination points fed into the funtion getPerspectivetransform().

To get the source_points and destination points, we get an image in vehilce_view of a road that we know that ave a parallel lanes(not curved). So we know that the four destionation_points will form a rectangle as the output of should have the lane parallel. I choose the source_points based on eyeballing the pixels on the lanes of the image in "vehicle_view" and adjusting based output of the derived transformation matrix.

A BirdsEye specifically for this. It take in source_points, destiantion_points, the camera_calibration_matrix and distortion_coefficients. You can use its undistort() function to output and undistorted image thata makes use of the camera_calibration_matrix and distortions_coefficients. 

FILTERING LANE PIXELS: GRADIENT AND COLOR THRESHOLDING  

