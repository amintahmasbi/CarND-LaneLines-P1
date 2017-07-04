# **Finding Lane Lines on the Road** 


**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

In the current pipeline, I am using the following steps to filter the lane lines:
- Grayscaling the image is the first step. Halfway through the project I noticed that I am using cv2.COLOR_BGR2GRAY while I am not using opencv's imread (matplotlib's imread is used). This issue was the source of low quality lane line detections. After using cv2.COLOR_RGB2GRAY the accuracy improved significantly
- Gaussian Blur smoothing is then used before edge detection to reduce number of false edge detections
- The image is then passed to  Canny Edge Detection to find all the edges. **The _low_ and _high_ thresholds of Canny are among the hyper-parameters that can affect the outcome of the algorithm.** Even though it is always a trade-off between lossing actual edges while trying to reduce the noisy edges (values in range of [50,90] and [150,190] are examined for low and high threshold, respectively.)
- Then the image is masked by a region of interest, since we are not expecting to find lane line in the air! (with a correctly calibrated camera). At some point in the middle of the project I noticed that the order of vertices of the ROI quadrilateral was not correct. As a result, a butterfly was created instead of the quadrilateral and I would miss most of my lines with that mask. After fixing the issue I could detect the major lines on the road.
- Hough Tranform line detection, is then used to take out the most significant lines from the Canny output in the ROI. **Accurate determination of parameters like _max-line-gap, min-line-len, threshold_ for the hough lines function, plays a significant role on output of the whole pipeline.** It is again a trade-off over fine-tuning such parameters as defferent environments might need different values.
- Finally, the lines from Hough Transform where sorted and seperated into left and right lines based on their **slopes**. The slope and y-intercept of both lines on each side were averaged to determin the single left and right lane lines.

### 2. Identify potential shortcomings with your current pipeline
Few eratical frames exist in the second video due to horizontal road crackes and noise lines in those frames. Those errors could be eliminated by bounding the lane line slopes on each side.

### 3. Suggest possible improvements to your pipeline

A possible improvement would be to warp the image and then find the lines. Refer to [Advanced Lane Lines Project](https://github.com/amintahmasbi/CarND-Advanced-Lane-Lines)

---
### Reviews

####1. The output video is an annotated version of the input video
Really excited when reading your code, I quite enjoyed how you tested various parameters on the images, the reduced region of interest, and also the "Hough Lines" test! You've managed to successfully test various values for the parameters and chosen the correct values for optimum output, your video "white.mp4" gives me the assumption that the value you use for the gaussian_blur function in particular "kernel_size" = 5, you have also realised that it being odd is inadmissible, this is because the value 3, 5, 7, 9... for this particular domain blurs the image to the right amount for the Gaussian Filter to be optimum. However, to reduce noise, I would slightly reduce the kernel_size value to 3.

Anyway, you may read more about the OpenCV API at: http://docs.opencv.org/2.4/modules/imgproc/doc/filtering.html?highlight=gaussianblur#gaussianblur

Not many students are able to ensure that the "left-broken lane lines" for the video "white.mp4" or "yellow.mp4" produce a connected Hough Line, but you have, which is quite intriguing :) even though a few minor changes for your parameter values like your min_line_length = 50, max_line_gap = 40 has a inconsistency, i.e. how can you assume that the lane line will be a small threshold of 50? in length, a threshold would require you to test a set of values within a min and max threshold, so a more suitable value for these variables would be, say: min_line_len = 100, max_line_gap = 160.

Overall, a really good solution and you have met the requirements for this specification!

####2. Visually, the left and right lane lines are accurately annotated by solid lines throughout most of the video.
Not much to say about your draw_line algorithm you haven't made much of a change, however, your algorithm elsewhere works quite efficiently :)

I would definitely recommend reading the python programming conventions: https://www.python.org/dev/peps/pep-0008/ just to understand the foundation of python programming, especially when it comes to repeating code when you could just make functions with various parameters and just call them assigning them to various types of the same objects,

Regarding parameter values and in the future, try to test various values within thresholds for the domain you're coding for to never miss out a particular optimal output.

Not much to say about this requirement, you have passed it with flying colours and also I have elaborated a little more on the comments above :)

####3. Reflection describes the current pipeline, identifies its potential shortcomings and suggests possible improvements. 

Amazing theoretical analysis at the end of your notebook submission, it's like reading Shakespeare but you explaining every inch of your code, I read all of it but you've blown me away, I would like to focus on a particular point you've made: "Few eratical frames exist in the second video due to horizontal road crackes and noise lines in those frames." and you're right, we could use colour maps from the Open CV api which detects a change in colour within the region_of_interest and performs depending on the colour, for animals it can be heat, for snow also: http://docs.opencv.org/2.4/modules/contrib/doc/facerec/colormaps.html

Not much to say about this requirement, you have passed it with flying colours and also I have elaborated a little more on the comments above :)