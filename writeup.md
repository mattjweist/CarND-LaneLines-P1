# **Finding Lane Lines on the Road** 

## Writeup

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report

### Reflection

### 1. Pipeline description

The pipeline begins by determining color thresholds to identify and mask colors appropriately. To identify yellow, orange, and white lane lines while masking the gray/black road, the RGB Color Codes Chart was referenced: https://www.rapidtables.com/web/color/RGB_Color.html. Next, a grayscale was applied to put the image in one color channel (gray). Then, gaussian filtering was applied to the image to reduce "noise" and remove extraneous pixels, making the lines smoother. Canny edge detection was then applied to detect the edges of the lines. The region of interest consists of a polygon enclosing the lane lines; the rest of the image was masked. Next, a Hough transformation was applied to image to generate an array of line endpoints. These endpoints were then displayed as lines using the draw_lines() function. These lines were then combined with the original image.

In order to draw a single line on the left and right lanes, the draw_lines() function was modified by sorting the lines output by the Hough transformation. The lines were sorted by slope; those with negative slope were associated with the right side, while those with positive slope were associated with the left side. Lines with relatively low slope were filtered out so as to not skew the data. Additionally, lines with "incorrect" slopes were filtered out; those with negative slopes on the right side and positive slopes on the left side of the image were removed. The slopes were then averaged, and the average position of the midpoints of the lines were calculated: thus an average x,y, and slope were calculated for both the left and right side. Using these calculated data and the knowledge that the lines should extend in the y-direction to the bottom of the image and to the top of the polygon applied earlier for region masking, a line for each side was generated.

[//]: # (Image References)
[image1]: ./test_images_output/solidWhiteCurve.jpg "Solid Lines"


### 2. Potential shortcomings

Many shortcomings with using camera footage stem from visibility issues. 

One potential shortcoming would be what would happen when lane lines are not available. This may seem obvious, but in many driving scenarios the roadlines are not present or evidently distinguishable.

Another shortcoming could be with steep hills or valleys. In the situation in which the driver is at the top of a steep hill , visibility of the lane lines could be limited. When the driver is at the bottom of a hill, visibility would also be limited.

As shown in the challenge video, the straight lane lines don't accurately follow all road features (like curves). 


### 3. Possible Improvements

The video shows that the pipeline produces relatively shaky output, especially on the side with a dashed line. To improve this, a smoothing method could be implemented by instead displaying the lines calcualted by averaging the lines of the current frame with those of the previous frame(s). The frame-per-second frequency is great enough to not skew data too noticably when implementing such a method. 
