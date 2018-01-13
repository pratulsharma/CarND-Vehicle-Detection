
**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./examples/car_not_car.png
[image2]: ./examples/HOG_example.png
[image3]: ./examples/sliding_windows.png
[image4]: ./examples/sliding_window.png
[image5]: ./examples/bboxes_and_heat.png
[image6]: ./examples/labels_map.png
[image7]: ./examples/output_bboxes.png
[video1]: ./project_video.mp4

## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Vehicle-Detection/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

### Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

The code for this step is contained in the first three code cell of the IPython notebook.

I started by reading in all the `vehicle` and `non-vehicle` images.  Here is an example of one of each of the `vehicle` and `non-vehicle` classes:

![alt text][image1]

- I explored different color spaces and different `skimage.hog()` function
- parameters used 
    - `orientations` = 9
    - `pixels_per_cell`= (8,8)
    - `cells_per_block` = (2.2)

I grabbed random images from each of the two classes and displayed them to get a feel for what the `skimage.hog()` output looks like.

![alt text][image2]

#### 2. Explain how you settled on your final choice of HOG parameters.

To find which color space to apply HOG, I use `get_hog_features()` function to return an image equivalent of extracted features, and visualized how recognizable the output image is? I experimented with different hog parameters and finally decided the parameters (as in previous comment) that gave high test accuracy for the SVM classifier.
The code for this is under `Final HOG` cell

#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

- I trained a linear SVM using `svm.SVC()` function. 
- The images were converted from RGD to YCrCb color space.
- The vectors included hog, color and color histogram features. 

### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

- The code for this step can be found under Final Pipeline cell.
- The search window was restriced between 370 to 600 along the y-axis. 
- The window size and overlap where the same as those used in the class notes.

![alt text][image3]

#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

- Several iteration were performed to get the parameters like hog, spatial and histogram color features right for the classifier. 
- To reduce false positives, threshold was applied. 
- Following images show the bounding boxes and heast maps of detected cars.

![alt text][image4]
---

### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)
Here's a [link to my video result](./project_video_output.mp4)


#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions.  I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  

- To improve false detection threshold was applied on integrated heatmap.
- To improve vehicle detection a deque with bounding boxes for 27 frames was applied.

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

- The pipeline still detects some false positives. I could possibly use add region masking to ignore the vehicles coming on the opposite lanes.
- When the two cars are close, the window momentorily moves only to one car. This could possibly be improved by more granular heatmap detection (two, instead of one) which would enable detection of two cars instead on one. 
- Playing with the size of window might help too.




