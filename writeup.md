# **Finding Lane Lines on the Road** 
### Author: Michael Shalala
---

[//]: # (Image References)

[solidWhiteCurve]: ./test_images_output/solidWhiteCurve.jpg "Example1"

**Finding Lane Lines on the Road**


The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on the work in a written report


The implementation is provided in [`P1.ipynb`](P1.ipynb) Jupyter notebook. The resulting images and videos are [`test_images_output`](./test_images_output) and 
[`test_videos_output`](./test_videos_output) correspondingly.

Example result:

![solidWhiteCurve]

---

### Details of the pipeline

My pipeline consists of 5 steps applied on an image:
* Convert the image to grayscale.
* Apply guassian-blur to reduce noise and ready the image for Canny edge detection.
* Apply Canny edge detection.
* Mask unwanted regions of the image (the relevant part of the road becomes smaller the farther we are from the point of view).
* Apply Hough Transform on the image to find the lines in our region of interest.
* Draw these lines on the original image.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function as follows,
* Calculate slope and bias and use it to:
  * Filter out lines that are not in the general direction of the road ahead.
  * Separate between left and right lane markings.
  * Average left and right lines to have best estimate for each.
* For videos, a moving average of the slope and bias is used to provide a more stable behaviour.


### Reflection

### 1. Potential shortcomings with the current pipeline

The current implementation is a naive one, it can serve as a proof of concept, but as can be seen from the challenge results, this pipeline is not robust enough to tackle the many problems of real-world cases:
* Lighting, reflections, shadows, all of them can easily confuse the pipeline, and there's nothing in the pipeline that handles errors.
* Dirt on the wind-shield, or on the camera-surface is another issue with the pipeline that adds noise.
* I haven't tried, but I don't think this pipeline will work well at night.
* Again, I haven't tried, but I don't think this pipeline will work with objects on the road that can obscure the view (such as cars for an example).


### 2. Possible improvements to the pipeline

* Improve the region of interest to be more accurate, based on camera position, elevation, etc...
* Error detection when the Canny image doesn't find enough edges (or finds too many of them).
* Improve the logic of where the line markings should be in the first place (a better probabilistic calculation).
* A lot more examples (especially adversarial one) to test the pipeline.
