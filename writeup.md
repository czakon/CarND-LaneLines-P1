# Finding Lane Lines on the Road

## Objective:

[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[image_cs]: ./intermediate_images/color_select_solidYellowCurve.jpg "final"
[image_final]: ./intermediate_images/final_solidWhiteCurve.jpg "final"

* To make a pipeline that finds the lane lines in images of roads. Using Canny edge detection and Hough transforms. The output will look like the image below.

![alt text][image_final]
---

### Reflection

###1. The main steps of my pipeleine are included in the `find_lines` function (not in `process_image`). There are seven major steps to the pipeline:
1. Color selection.
2. Defining the region of interest.
3. Grayscaling. 
4. Gaussian smoothing.
5. Canny edge detection. 
6. Hough transform line detection.
7. Draw the lines on the original image. 

#### Color Selection

The first step is to select only the white and the yellow colors in the image. Because white is the optical combination of red, green, and blue, but yellow is only the combination of red and green, I remove all of the blue from my image. To make this cut I have implemented a function called `color_selection`. The final image looks likes this:

![alt text][image_cs]



###2. Identify potential shortcomings with your current pipeline
One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


###3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
