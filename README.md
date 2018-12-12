# Stop Sign Recognition

All source code can be found here: [https://github.com/2K2A/final_matlab](https://github.com/2K2A/final_matlab)

Our test set of 40 images (20 positive, 20 negative) pulled off of random websites: [https://drive.google.com/file/d/1mwfzq5RjrL2qWeSt9AWhiR95ZiDzp_kz/view?usp=sharing](https://drive.google.com/file/d/1mwfzq5RjrL2qWeSt9AWhiR95ZiDzp_kz/view?usp=sharing)

## Members

Albert Latham (latham2@wisc.edu) NetID: latham2

Azhar Siddiqui (siddiqui4@wisc.edu) NetID: siddiqui4

Kendel Chopp (kchopp@wisc.edu) NetID: kchopp

Guk Kim (gkim95@wisc.edu) NetID: gkim95

## Problem
### What is the problem?
* Analyzing images/videos to determine whether or not there is a stop sign in the image/video
* Checking how far away the stop sign is if there is a stop sign

### Why does this problem matter?
* Autonomous vehicles need a way of detecting street signs, more specifically stop signs so they know when/where/if to stop. These vehicles are important as if they work well they will be able to decrease traffic, increase efficiency, safety, etc. all while allowing drivers to sit back and relax
* If a stop sign is detected and a driver is not slowing down, the car could assist in stopping or give the driver a warning in order to prevent potential accidents
* A system for stop sign detection could be used by insurance companies (or other parties) to detect how "good" a driver is. (e.g. If a stop sign is seen, does the driver come to a complete stop, is it abrupt, etc.) Similar practices are already in play, check out [Progressive's Snapshot](https://www.progressive.com/auto/discounts/snapshot/)

## Solution
### What is the current state-of-the-art?
* [https://ieeexplore.ieee.org/document/5716193](https://ieeexplore.ieee.org/document/5716193)
* [https://www.sciencedirect.com/science/article/pii/S0031320309002180](https://www.sciencedirect.com/science/article/pii/S0031320309002180)
* [https://arxiv.org/abs/1805.04424](https://arxiv.org/abs/1805.04424)

In autonomous vehicles, the state of the art is using convolutional neural networks. The companies developing these algorithms have the luxury of loads of data, and their datasets continue to expand as they drive their autonomous vehicles around.

That being said, when it comes to sign detection alone, it has been found that doing a hough transformation along with some color detection is sufficient for many different signs. Stop signs especially have a unique octagon shape along with a distinct red color that make them easier to identify by simply looking for their shape. Not to mention, convolutional neural networks have been found to have some issues with tampering (https://www.digitaltrends.com/cars/self-driving-cars-confuse-stickers-signs/) In addition, for something like an app for an insurance company, it is not necessary to detect all of the same things an autonomous vehicle needs to, and a simple sign detection algorithm may be a more practical solution for a company looking to implement something like this.

### Evolution of our plan
We have had numerous plans of attack throughout the process of building our solution.

#### Original Idea
Build and train a neural network to classify images as yes they have a stop sign or no they don't. We went away from this idea though due to the complexity of creating and training a neural network, as well as the fact that we would need a very large training set of images in order to get decent results.

#### Midterm Idea
At the midterm of the project we planned on following [this paper](https://ieeexplore.ieee.org/document/5716193) rather closely which just uses a simple hough transform to find lines around red objects and counts the number of lines. We did originally try to implement this, and can be found in the source code of our final project here: [https://github.com/2K2A/final_matlab/blob/master/hasEightSides.m](https://github.com/2K2A/final_matlab/blob/master/hasEightSides.m)
Despite much tinkering with parameters and filters and other methods, we were unable to get reliable results. Sometimes the same side would be recognized twice, sometimes sides wouldn't be recognized at all.

#### Final Idea
Our final idea, and what we ended up rolling with was using a combination of multiple methods. Each method returns a confidence score on a scale of 0-100 and we combine all of these scores to make them a composite score out of 300 which we then turn into a percentage confidence that there is a sign in the image. If it is more than 0.5 then we say there is a stop sign, if not then we say there isn't.

### Our Implementation
#### Framework
##### Main Script
###### Source Code
[https://github.com/2K2A/final_matlab/blob/master/main.m](https://github.com/2K2A/final_matlab/blob/master/main.m)

###### What It Does
The main script simply runs our test has_stop_sign on all images in both signs/true and signs/false where the folders contain images with stop signs and images without stop signs respectively.
The results are saved in results which records true positives, false negatives, false positives, and true negatives in that order.
It also calculates an overall accuracy by taking the true positives + true negatives divided by the total number of images

##### Has Stop Sign
###### Source Code
[https://github.com/2K2A/final_matlab/blob/master/hasStopSign.m](https://github.com/2K2A/final_matlab/blob/master/hasStopSign.m)

###### What It Does
has_stop_sign takes an image as input and decides whether or not there is a stop sign in the image.
The criteria are 3 tests each out of 100 (template matching, SIFT, and white pixel ratio)
The image can also earn bonus points if OCR picks up a word that resembles STOP, but since it does not tell us much if OCR fails and only if it succeeds, we do not consider it a standard test and only a bonus.
If it scores at least 150/300 (50%) then we say there is a stop sign in the image, otherwise there is not.

#### Tests
##### Test 1: Template Matching
###### Source Code
[https://github.com/2K2A/final_matlab/blob/master/matchesTemplate.m](https://github.com/2K2A/final_matlab/blob/master/matchesTemplate.m)

###### How It's Done
1. Find the largest red blob in the image by filtering out non-red objects and using bwlabel to find connected components and then sorting them by their area
2. Put a bounding box around the red blob
3. Create a regular octagon and morph it to fit into the bounding box
4. Check how many pixels from the regular octagon and original image match and are different
5. Take the number of different pixels / size of bounding box to get a percentage different
6. After trial and error, it was found that any difference > 20% is very rarely a stop sign, so get a score based on how different it is out of the 20% and scale to be out of 100

###### Drawbacks
* By only using the largest red blob, other objects like billboards, cars, etc. can get in the way. This can be circumvented by checking the largest n blobs, or all blobs greater than a given size
* Rotated stop signs or strange perspectives do not match very well
* "All Way" or "4 Way" sign additions on the bottom make it less accurate, but it is still usually pretty accurate
* Other red objects touching the stop sign (e.g. a red flag hanging off of the stop sign becomes a part of the blob)

###### Results
Efficiency: Roughly 0.2 seconds/image varying depending on image size

Our Test Set: 77.5% accuracy

| True Positives | False Negatives | False Positives | True Negatives |
|----------------|-----------------|-----------------|----------------|
| 12             | 8               | 1               | 19             |


Original With Bounding Box            |  Template on top of red blob
:-------------------------:|:-------------------------:
<img src="https://raw.githubusercontent.com/2K2A/2K2A.github.io/master/images/template_matching/good_box.jpg" alt="good_box" width="200"/>  |  <img src="https://raw.githubusercontent.com/2K2A/2K2A.github.io/master/images/template_matching/template_good_pair.jpg" alt="good_template" width="200"/>
<img src="https://raw.githubusercontent.com/2K2A/2K2A.github.io/master/images/template_matching/bad_box.jpg" alt="bad_box" width="200"/>  |  <img src="https://raw.githubusercontent.com/2K2A/2K2A.github.io/master/images/template_matching/template_bad_pair.jpg" alt="bad_template" width="200"/>

##### Test 2: SIFT/Feature Matching
###### Source Code

[https://github.com/2K2A/final_matlab/blob/master/featureMatch.m](https://github.com/2K2A/final_matlab/blob/master/featureMatch.m)
[https://github.com/2K2A/final_matlab/blob/master/getStopSign.m](https://github.com/2K2A/final_matlab/blob/master/getStopSign.m)

###### How It's Done

Two images are processed in tandem, an image that we are trying to find a stop sign in, and a template stopsign image.

<img src="https://raw.githubusercontent.com/2K2A/2K2A.github.io/master/images/feature_matching/stop_sign_template.jpg" alt="good_box" width="200"/>
<img src="https://raw.githubusercontent.com/2K2A/2K2A.github.io/master/images/feature_matching/img.jpg" width="200"/>

1. `getStopSign.m` is called and takes an image as an argument and returns a cropped image of the largest red thing present.

<img src="https://raw.githubusercontent.com/2K2A/2K2A.github.io/master/images/feature_matching/sign.jpg" width="200"/>

2. Then, the images are blurred (de-noising).

<img src="https://raw.githubusercontent.com/2K2A/2K2A.github.io/master/images/feature_matching/sign_blur.jpg" width="200"/>
<img src="https://raw.githubusercontent.com/2K2A/2K2A.github.io/master/images/feature_matching/temp_blur.jpg" width="200"/>

3. Edges are found using a Canny edge-finder.

<img src="https://raw.githubusercontent.com/2K2A/2K2A.github.io/master/images/feature_matching/sign_edge.jpg" width="200"/>
<img src="https://raw.githubusercontent.com/2K2A/2K2A.github.io/master/images/feature_matching/temp_edge.jpg" width="200"/>

4. Features are found using the [VL_SIFT](http://www.vlfeat.org/matlab/vl_sift.html) algorithm and matched using [VL_UBCMATCH](http://www.vlfeat.org/matlab/vl_ubcmatch.html).

<img src="https://raw.githubusercontent.com/2K2A/2K2A.github.io/master/images/feature_matching/matched_features.jpg" width="200"/>

5. To score the match, the scores returned by `VL_UBCMATCH` are treated and returned:

```Matlab
  score = 1/sqrt(sum(scores)/max(scores))*100;
```

###### Drawbacks
* This method is slow (see Results section below).
* This method *only* considers the largest red blob.
* The features that are 'matched' aren't matching features.

###### Results
Efficiency: Roughly 0.8 seconds/image varying depending on image size

Our Test Set: 40% accuracy (Low accuracy on this test, but much higher when the stop signs are farther away)

| True Positives | False Negatives | False Positives | True Negatives |
|----------------|-----------------|-----------------|----------------|
| 5             | 15               | 9               | 11             |

##### Test 3: Ratio Scoring
###### Source Code
[https://github.com/2K2A/final_matlab/blob/master/detectStop.m](https://github.com/2K2A/final_matlab/blob/master/detectStop.m)

###### How It's Done
1. Find the interesting red blob in the image (originally just the largest, later move onto a better heuristic. See alternative solution)
2. Fill in the red blob
3. Find the number of pixels that were filled in (what should be the letters "STOP")
4. Calculate the ratio of inside pixels to the number of pixels in the blob
5. Check how close that ratio is to an image we know to have a stop sign

###### Drawbacks
* It is dependent on us having a good ideal value
* Originally, by looking at the largest red blob we missed the stop sign for things like cars and buildings or other random red objects
* Low resolution images do not have enough inner pixels to fill in

###### Results
Efficiency: Roughly 0.01 seconds/image varying depending on image size

Our Test Set: 80% accuracy (High efficiency and accuracy, likely the best individual test)

| True Positives | False Negatives | False Positives | True Negatives |
|----------------|-----------------|-----------------|----------------|
| 14             | 6               | 2               | 18             |

##### Test 4: OCR (Character Recognition for 'STOP')
###### Source Code
[https://github.com/2K2A/final_matlab/blob/master/hasWordStop.m](https://github.com/2K2A/final_matlab/blob/master/hasWordStop.m)

###### How It's Done
1. Detecting text regions in an RGB image using MSER feature of Matlab by optimizing text detection using image features such as good color consistency and high contrast text
2. Removing the non-text regions in the image using geometric properties of text (eccentricity, euler, solidity, extent, etc.) after analysing MSER feature results
3. Using stroke width variation as an extra feature to remove non-text regions in the image. The stroke width of a MSER detected region containing human readable text has little variation over most part of the region. Hence, by applying threshold values the variation over an entire MSER detected region is reduced to a single metric. This is done for all MSER detected regions to further remove non-text regions from the image.
4. Since the detection results at this point are individual text characters, the characters are merged into text lines for OCR recognition. This is done by constructing bounding boxes around individual text characters and expanding bounding boxes using regionprops. Once the bounding boxes overlap the neighboring bounding boxes, by the use of overlapping ratio and using the graph function finds all the text regions connected by a non-zero overlap. Finally, all overlapping neighboring bounding boxes are merged into a single bounding box ready for OCR recognition.
5. Simply using the OCR function helps to detect the text within each bounding box.

###### Drawbacks
* Due to low image resolution OCR function is unable to detect words inside a text region clearly. This is because ideal use of OCR function without enhancements is for stationary text instead of detecting text in live images.
* If a stop sign in an image is too close (making the word "STOP" heavily enlarged) or at too far a distance (making the word "STOP" minuscule) OCR function is unable to detect any text inside a clearly defined bounding box. To fix this, enhancements have to be made to the default OCR function to make it able to recognise texts at a variety of depths.
* If a stop sign is dusty or pixelated in an image OCR will not be able to recognise text correctly.


###### Results
Efficiency: Roughly 0.6 seconds/image with higher variability than the other tests

Our Test Set: 77.5% accuracy

| True Positives | False Negatives | False Positives | True Negatives |
|----------------|-----------------|-----------------|----------------|
| 11             | 9               | 0               | 20             |

<img src="https://raw.githubusercontent.com/2K2A/2K2A.github.io/master/images/ocr/1.png" alt="good_box" width="200"/>  |  <img src="https://raw.githubusercontent.com/2K2A/2K2A.github.io/master/images/ocr/2.png" alt="good_template" width="200"/>
:-------------------------:|:-------------------------:
string = 'STOP'  |  string = '\[STOP]E'
<img src="https://raw.githubusercontent.com/2K2A/2K2A.github.io/master/images/ocr/3.png" alt="good_box" width="200"/>  |  <img src="https://raw.githubusercontent.com/2K2A/2K2A.github.io/master/images/ocr/4.png" alt="good_template" width="200"/>
string = '\[STOP-I'  |  string = 'ISTOPI'

#### Alternative Solution
When creating the slides and website for our implementation, we stumbled upon a new solution that provided higher accuracy on our test set and a significant improvement in efficiency.

##### Source Code
* Get the best red blob [https://github.com/2K2A/final_matlab/blob/master/getRedBlob.m](https://github.com/2K2A/final_matlab/blob/master/getRedBlob.m)
* Get the template score on a given blob (template matching) [https://github.com/2K2A/final_matlab/blob/master/getRedBlob.m](https://github.com/2K2A/final_matlab/blob/master/getRedBlob.m)
* Get the ratio score of a given (best) blob [https://github.com/2K2A/final_matlab/blob/master/ratioCheck.m](https://github.com/2K2A/final_matlab/blob/master/ratioCheck.m)

##### What is it?
We used a combination of Template Matching and our Ratio Checking
1. Find all of the red connected components
2. For every red connected components run the template matching algorithm to find the red blob that is the most octangular.
3. Take that connected component and do the ratio test on that individual component

## Overall Results
### Original Solution
Efficiency: Roughly 1.5 seconds/image

Our Test Set: 82.5% accuracy

| True Positives | False Negatives | False Positives | True Negatives |
|----------------|-----------------|-----------------|----------------|
| 15             | 5               | 2               | 18             |

### Alternative Solution
This solution tested multiple red blobs and defined the red blobs more closely, allowing for better matchings, and the ability to not worry about things like red flags or buildings in the background.

Efficiency: Roughly 0.05 seconds/image (Allows for 20 FPS video)

Our Test Set: 97.5% accuracy

| True Positives | False Negatives | False Positives | True Negatives |
|----------------|-----------------|-----------------|----------------|
| 20             | 0               | 1               | 19             |

## Future
* Run the recognition on a lower resolution dashcam video to detect stop signs live (Will also need to speed up tests or remove tests that do not perform as well)
* Use the size of the stop signs and perspectives to determine how far the car is from the stop sign
* Use similar recognition tactics to detect other street signs
