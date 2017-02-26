# Finding Lane Lines on the Road

## Objective:

* To make a pipeline that finds the lane lines in images of roads. Using Canny edge detection and Hough transforms.

[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[image_final]: ./intermediate_images/final_solidWhiteCurve.jpg "final"

---

### Reflection

###1. My main steps of my pipeleine are included in the `find_lines` function. I've made this general enough that I don't use the name `process_image`. There are seven major steps to the pipeline:
1. Color selection.
2. Defining the region of interest.
3. Grayscaling. 
4. Gaussian smoothing.
5. Canny edge detection. 
6. Hough transform line detection.
7. Draw the lines on the original image. 

##### Color Selection




In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]
![alt text][image_final]

###2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


###3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
