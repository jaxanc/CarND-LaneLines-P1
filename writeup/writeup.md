# Report for Finding Lane Lines on the Road

[//]: # (Image References)

[original]: ./solidWhiteCurve.jpg "Original Image"
[step1]: ./step1_color_select.jpg "Colour Select"
[step2]: ./step2_grayscale.jpg "Grayscale Transform"
[step3]: ./step3_canny_transform.jpg "Canny Transform"
[step4]: ./step4_region_of_interest.jpg "Region of Interest"
[step5]: ./step5_hough_transform.jpg "Hough Transform"
[step6]: ./step6_draw_on_original_image.jpg "Combined with Original"

## 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps, as described in the following sections. The original image is:
![alt text][original]

### 1.1 Colour Select
Only the white and really yellow colours are selected from the original image. Anything with a slight blue is removed. This has shown to be effective on the `challenge.mp4` for isolating the lane when travelling over highly illuminated road section. The "bright" road section is slightly blue when compared to the white (and yellow) lane markings.
![alt text][step1]

### 1.2 Grayscale Transform
The colour selected image is then turned into grayscale for the Canny edge detection function. However, even if I didn't turn the image into grayscale; the results are the same. My original thought is to process the white and yellow colours separately to help improve detection. I have yet to investigate into this.
![alt text][step2]

### 1.3 Canny Transform
Then Gaussian blur and Canny transform is applied. The same steps (and code) as taught during the lessons.
![alt text][step3]

### 1.4 Region of Interest
Region of interest is created by marking 4 lines. The shape is chosen by trial and error and relative to the number of pixels as required by the optional challenge
![alt text][step4]

### 1.5 Hough Transform
Apply Hough transform to image. The resulting lines from the Hough transform are  passed into the modified `draw_lines()` function to draw only two lines.
![alt text][step5]
In order to draw a single line on the left and right lanes, I modified the `draw_lines()` function by spliting all the lines by their x-axis locations. Then lines on the left and right are split into individual points and a single order  less square fitting is applied to obtain the averaged two single lines.

### 1.6 Apply to Original Image
Draw the two lanes onto original image using the provided function
![alt text][step6]

## 2. Identify potential shortcomings with your current pipeline

### 2.1 Region of Interest is Fixed
The region of interest are determined by trial and error on the provided images and videos. Because it is fixed it does not work well if the camera is mounted at a different location (dashboard and back mirror would be in field of view), the cars are travelling down hill or making a big turn.

### 2.2 Colour Selection is Fixed
If the brightness or contrast of the images and video changes, the thresholds will not be able to cope. They can be pre-processed to have similar brightness or the thresholds need to adjust.

### 2.3 Lanes are Selected by Their x-axis Location
The number of votes are selected relatively low to obtain as many points as possible, for the purpose of combining the points for least square fitting. The down side is that the lines do not have consistent gradients to be used for separation. If the cars are making a significant right turn, the lanes could be crossing the centre line and be mistaken as part of the other lane.

### 2.4 Linear Regression is Used to Fit a Curved Line
The lanes are approximated as linear lines but a lot of the times they are curved. It is difficult to fit this as a minimum 3rd order polynomial would be required to correctly for distance and altitude. The potential of high order polynomial to run wild is high. A better way might be to define the curve analytically and fit the points around it.

### 2.5 Not Enough Lines Recognised From "None Solid" lanes
When the lanes are curved, more lines will be recognised in the distance (appears more solid) and if dotted lines are not correctly picked up near the camera, the linear line will change gradient rapidly.

### 2.6 Hard to Distinguish Between Lanes and Other White and Yellow Objects On the Road
Because gradient of the lines are not being used to determine lane markings. This pipeline is more susceptible to pick up similar colour objects on the road.

## 3. Suggest possible improvements to your pipeline

### 3.1 Average Lane Markings From Previous Frames
Lane markings from previous frames can be used to help determine lanes in the current frame. The number of averages or filtering time constant can be determined as a function of framerate and travelling speed.

### 3.2 Fit Lane Markings with a Curve
1st order polynomial fit does not produce a good fit on curved roads. Analytical expression or higher order polynomial can be used to determine the correct lane markings.

### 3.3 Use Other Sensors
If another car or big truck is in front of the camera; or when the car is reaching traffic lights and roundabouts. The calculated lane markings are not deterministic. Other sensors can be used to help assess the situation. Such as proximity sensors or GPS
