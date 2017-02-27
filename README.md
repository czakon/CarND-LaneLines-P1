# Finding Lane Lines on the Road

## Objective:

[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"
[image_cs]: ./intermediate_images/color_select_solidYellowCurve.jpg "final"
[image_roi]: ./save/roi_solidYellowCurve.jpg "roi"
[image_final]: ./intermediate_images/final_solidWhiteCurve.jpg "final"

* To make a pipeline that finds the lane lines in images of roads. Using Canny edge detection and Hough transforms. The output will look like the image below, and will work for movies as well.

![alt text][image_final]
---

### Reflection

###1. The main steps of my pipeleine are included in the `find_lines` function (not in `process_image`). There are eight major steps to the pipeline:
1. Color selection.
2. Defining the region of interest.
3. Grayscaling. 
4. Gaussian smoothing.
5. Canny edge detection. 
6. Hough transform line detection.
7. Extracting the parameters for the left- and the right-lane lines
8. Draw the lines on the original image. 

#### 1. Color Selection

The first step is to select only the white and the yellow colors in the image. Because white is the optical combination of red, green, and blue, and yellow is the combination of red and green, I remove all of the blue from my image, and retain only pixels where both the red and the green is above a value of `180`. To make this cut I have implemented a function called `color_selection`. The final image looks likes this:

![alt text][image_cs]

#### 2. Defining the region of interest

To exclude noise from the images, I masked all of the pixels in the image that were outside a trapezoidal region of interest, that included the optical distortion of the lane lines as viewed on the horizon. I defined this region of interest using three parameters: `xfract_t = 0.45`, `xfract_b = 0.05`, and `yfract = 0.60`. The y-span of the trapezoid starts `yfract` down the image, and the extends to the bottom. The x-limits of the region of interest cut out the first and last `xfract_b` at the bottom, and the first and the last `xfract_t` at the top. In other words, the top 60% of the image is excluded. Of what remains, I cut off 5% of the image on the left- and the right-hand sides at the bottom, and I cut of 45% of the image from the left- and right-hand sides at the top. An example of the region of interest being selected from a raw image is below:

![alt text][image_roi]

#### 3. Grayscaling

In the following steps, the RGB image qualities of the image no long need to be preserve, and I reduce the image by 2 dimensions by implementing a grayscale this implements the OpenCV `cvtColor` option. 

#### 4. Gaussian smoothing

Small discontinuities in the image can be removed by Gaussian smoothing. For this, I have implemented the OpenCV `GaussianBlur` option using a kernel size of 9 pixels.

#### 5. Canny edge detection

Edges can be detected using the OpenCV `Canny` function, which uses gradient features in an image. Here, I use `low_threshold=50` and `high_threshold=200`. The `high_threshold` parameter defines how strong the gradient has to be identified as a line, and the `low_threshold` feature can be tuned to determine which neighboring pixels belong to the line. 

#### 6. Hough transform line detection

Using the detected edges, we can then detect lines in the edges by looking at their signature in Hough space. The parameters that I tuned for this part of the project are the distance resolution (`rho=1`), the angular resolution in degrees (`theta=pi/180`), the minimum number of votes to be detected (`threshold=10`), the minimum length of the line (`min_line_len=20`), and the maximum gap between pixels that can be connected (`max_line_gap=4`). I simply tuned these parameters until they worked on the test images, however, more that should be given to make sure that they are physically reasonable parameters.

#### 7. Extracting the parameters for the left- and the right-lane lines

My approach in this part of the project was to use the simplest and the fastest method to extract the lane lines for the image. Using the output from the Hough transform, I calculated what the slope and the intercept for each line segment should be. I then identified the two clusters of points, that represented the left- and the right-lane lines. I then defined a region around these lines that I could use as a prior for where I expect the lane lines to be located. Then, I took the average of the slope and the intercept in these two regions, to define where I believed the left- and the right-hand lane lines should be. For the right lane line, I constrain the slope to be between `[-1.0,-0.6]` and the intercept to be betweein `[500,800]`, and for the left lane line, I constrain the slope to be between `[0.5,0.7]` and `[-50,100]`. These were tuned until the program did not crash on the test videos, however, given that the slope and intercept regions have different sized ranges for the left- and the right-hand side, this could be improve to make sure that they are of the same size. 

#### 8. Drawing the lines on the image. 

Using the slopes and the interecepts for the extracted lane lines, I then find what the x coordinates correspond to in the top and the bottom of my region of interest. With the subsequent two edge points that I have for the right and the left-lane lines, respectively, I then draw two lines using the OpenCV `line` function. Prompted by the suggestions in the project description, I use the `weighted_img` function to combine the image with a solid line with the original image, so that the lane lines do not look as bright, and so that it is possible to see the street below. 

### Possible shortcomings and suggestions for improvement. 
My project failed on the challenge video. This, I believe, is because the hood of the car could be seen in the bottom part of the image. Because I put a prior on where I expect the left and the right lanes to be, I'm not exactly sure why this would crash my code, and this requires further investigation. Nevertheless, an obvious fix would be to cut out the bottom portion of the images in the future to prevent a mistake like this happening again.

Overall, I did not think very hard whether the parameter limits that I imposed are physically reasonable, and in the future, and I would want to double-check that they make physcial sense. 

I believe that that the lane lines in these test images where very bright and new. I would think that my process would fail if the lane lines are not so distinct. Or, if there are no lane lines, such as on a narrow road. 

The major shortcoming of this project is that it was only tested on a limited number of test images in ideal conditions. As the sample size of testing images increases, I imagine that my pipeline might fail if the road has very tight turns, goes over a hill, or is driving in regions where there is a lot of color variation, such as tree-lined street with lots of dark and bright patches. 
