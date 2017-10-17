**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image1]: ./output_images/3323.png "Car image"
[image2]: ./output_images/image2252.png "Not car image"
[image3]: ./output_images/hogFeatures.png "Hog features"
[image4]: ./output_images/windows.jpg "Sliding windows"
[image5]: ./output_images/boxes.jpg "Detected boxes"
[image6]: ./output_images/frame0.jpg "Heat map and boxes"
[image7]: ./output_images/frame1.jpg "Heat map and boxes"
[image8]: ./output_images/frame2.jpg "Heat map and boxes"
[image9]: ./output_images/frame3.jpg "Heat map and boxes"
[image10]: ./output_images/frame4.jpg "Heat map and boxes"
[image11]: ./output_images/frame5.jpg "Heat map and boxes"
[image12]: ./output_images/frame6.jpg "Heat map and boxes"
[image13]: ./output_images/frame7.jpg "Heat map and boxes"


## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

### Histogram of Oriented Gradients (HOG)

#### 1. Explain how (and identify where in your code) you extracted HOG features from the training images.

The code to identify the HOG features is contained in the function `get_hog_features()` written in the file carFinding.py.

I started by reading in all the `vehicle` and `non-vehicle` images. Here is an example of one of each of the `vehicle` and `non-vehicle` classes:

![alt text][image1]
![alt text][image2]

I then explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`).  I grabbed random images from each of the two classes and displayed them to get a feel for what the `skimage.hog()` output looks like.

Here is an example using the `YCrCb` color space and HOG parameters of `orientations=8`, `pixels_per_cell=(8, 8)` and `cells_per_block=(2, 2)`:

![alt text][image3]

#### 2. Explain how you settled on your final choice of HOG parameters.

I tried various combinations of parameters and I decided tho choose these ones because there were what produced the best classification results using the linear svm.

#### 3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).

I trained a linear SVM using features extracted from the car and non car image datasets provided in the lesson. The features used were:
* Spatial feature: It is a flattened array of the image channels
* Color feature: It is the histogram of the colors in the image
* Hog feature: Represents the direction of the gradients of the image

I concatenated these three features per image, I scaled them and finally I trained the linear SVM. The procedure is coded in the functions `bin_spatial()`, `color_hist()`, `get_hog_features()`, `extract_features()`, `getSampleFeatures()` and `trainClassifier()`.

### Sliding Window Search

#### 1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

I searched using four different window positions and scales all over the image. These positions were:

| y position         	|     Scale				| 
|:---------------------:|:---------------------------------------------:| 
|  380, 480         	| 1			   			| 
|  380, 530     	| 1.3 						|
|  380, 580     	| 1.6						|
|  380, 650     	| 2 						|


The next image shows the result of all windows superimposed.

![alt text][image4]

#### 2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

Ultimately I searched on four scales using YCrCb 3-channel HOG features plus spatially binned color and histograms of color in the feature vector, which provided a nice result.  Here is one example image:

![alt text][image5]
---

### Video Implementation

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)

Here's a [link to my video result](./projectVehicle_output.mp4)


#### 2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

I recorded the positions of positive detections in each frame of the video.  From the positive detections I created a heatmap and then thresholded that map to identify vehicle positions.  I then used `scipy.ndimage.measurements.label()` to identify individual blobs in the heatmap.  I then assumed each blob corresponded to a vehicle.  I constructed bounding boxes to cover the area of each blob detected.  

Here's an example result showing the heatmap from a series of frames of video, the result of `scipy.ndimage.measurements.label()` and the bounding boxes then overlaid on the last frame of video:

### Here are eight frames, their corresponding heatmaps and the final bounding boxes:

![alt text][image6]
![alt text][image7]
![alt text][image8]
![alt text][image9]
![alt text][image10]
![alt text][image11]
![alt text][image12]
![alt text][image13]



---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

The white car is more likely not to be detected than the black car, specially when it is far from the camera. I think this could be solved tunning a little bit more the color features.

