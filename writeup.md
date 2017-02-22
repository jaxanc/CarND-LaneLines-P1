# Report for Finding Lane Lines on the Road

## 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps:
1. Only the white and really yellow colours are selected from the original image. Anything with a slight blue is removed. This has shown to be effective on the `challenge.mp4` for isolating the lane when travelling over highly illuminated road section. The "bright" road section is slightly blue when compared to the white (and yellow) lane markings.
2. The colour selected image is then turned into grayscale for the Canny edge detection function. However, even if I didn't turn the image into grayscale; the results are the same. My original thought is to process the white and yellow colours separately to help improve detection. I have yet to investigate into this.
3. Then Gaussian blur and Canny transform is applied. The same steps (and code) as taught during the lessons.
4. Region of interest is created. This step can be

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...

If you'd like to include images to show how the pipeline works, here is how to include an image:



## 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ...

Another shortcoming could be ...


## 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
