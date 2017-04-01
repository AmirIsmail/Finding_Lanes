#**Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Modify the pipeline to adapt to diffrent lane environments
* Save the output images in the project directory to be reviewed
* After editing is satisfactory, apply pipeline on sample video which is a series of images
* Save the output video in in the project directory to be reviewed
* Modify the function that draws the lane lines to draw continuous lines instead of dashed lines
* Apply on both example videos untill satisfactory output  


[//]: # (Image References)

[image1]: ./test_image_output/solidWhiteCurve.jpg : Output of the pipeline 
[image2]: ./test_image_output/solidWhiteRight.jpg : Output of the pipeline 
[image3]: ./test_image_output/solidYellowCurve.jpg : Output of the pipeline 
[image4]: ./test_image_output/solidYellowCurve2.jpg : Output of the pipeline 
[image5]: ./test_image_output/solidYellowLeft.jpg : Output of the pipeline 
[image6]: ./test_image_output/whiteCarLaneSwitch.jpg : Output of the pipeline 


[//]: # (Video References)

[video1]: ./test_videos_output/solidWhiteRight : output of looping on the frames inside video and processing each with the constructed pipeline
[video2]: ./test_videos_output/solidYellowLeft : output of looping on the frames inside video and processing each with the constructed pipeline

---

### Reflection

###1. Pipeline Description:

My pipeline consisted of the following 6 steps: 
	1. I converted the images to grayscale
	2. I applied Gaussian smoothing with kernel size equal 3 to the grey-scaled image 
	3. I applied canny edge detection to get edges in the image
	4. I defined the region of interest as a polygon with the required vertices defining lanes(verices were adapted to fit the different image examples )
	5. I applied hough transform to connect the lines of the edges
	6. I draw the filtered lines on top of the original image.

In order to draw a single line on the left and right lanes I modified the draw_lines() by the following:
	1. I calculated the slope and center of each dashed line 
	2. I compared slope against zero to check if the current line belongs to the left or right lane (+ve slope represent left lane while -ve represent right lane, 
	also i ignored the vertical and horizontal slopes)
	3. I created 4 lists holding the values of the slopes and centers of the left lane dashed lines & the slopes and centers of the right lane dashed line
	4. I calculated the average slope of the left and right lane lines using the equation (np.sum(<right,left slope list>)/len(<right,left slope list>), np.divide(np.sum(<right,left center list>),len(<right,left center list>)))
	5. Based on the calculated average center and slope of each lane, I extrapolated the line end point for both lanes(end points are at the bottom and middle of the image), extrapolation was done using the following equation:
		(y-y`)=M(x-x`) --> x & y are substituted with the center point, M with the average slop and y with end points to extrapolate x` for both end points
	6. Lines are then drawn using the extrapolated end points
	



###2. Potential shortcomings with current pipeline:
	1. Curved road with continuous changing slope such that the draw_lines function will not be able to differentiate left and right lanes using the slopes only


###3. Suggested improvements to the pipeline:
	1. Identification that the line belongs to left or right lane in "draw_lines" funcion shouldn't only depend on slope as at some points the two lanes are both positive or negative so instead 
	of using slopes to differentiate we can define possible x range for the left and another for the right and compare the current x against this range to define left and right lane lines