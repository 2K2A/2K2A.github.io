# Stop Sign Recognition

## Members

Albert Latham (latham2@wisc.edu) NetID: latham2

Azhar Siddiqui (siddiqui4@wisc.edu) NetID: siddiqui4

Kendel Chopp (kchopp@wisc.edu) NetID: kchopp

Guk Kim (gkim95@wisc.edu) NetID: gkim95
## 1) Briefly explain what problem you are trying to solve.
* Analyzing road signs and deciding whether a moving vehicle should stop when it detects a road sign. The driver will be notified if a stop sign is encountered in the vehicle’s proximity, otherwise, no warning is generated for the driver.
* Other caveats involved may include checking how far away the stop sign is depending on the size of the stop sign within the image. This means we may want to warn drivers to slow down when the stop sign is approaching and warn with a more serious “stop” when the stop sign is close.


## 2) Why is this problem important? 
* If stop signs are detected and the information is relayed to a driver this could result in decreasing the incidence of traffic accidents and potentially save lives.
* Solving this problem would allow an alarm that could be used to keep drivers alert and prevent them from dozing off at the wheel or simply missing stop signs.
* This solution can also be used as part of an autonomous vehicle’s algorithms. If the solution is fast enough it could be used to pass information to more complex (and possibly slower) algorithms. Autonomous vehicles have a whole host of benefits including decreasing traffic, increasing efficiency, safety, etc. A simple, fast, and effective algorithm would allow for cheaper hardware and more flexibility on the part of engineers.
* A system like this could also be used by insurance companies (or other parties) to detect how “good” a driver is. A car could detect if a stop sign was seen, and whether or not the driver stopped and/or how abruptly the driver stopped.


## 3) What is the current state-of-the-art? 
* https://ieeexplore.ieee.org/document/5716193
* https://www.sciencedirect.com/science/article/pii/S0031320309002180
* https://arxiv.org/abs/1805.04424 

In autonomous vehicles, the state of the art is using convolutional neural networks. The companies developing these algorithms have the luxury of loads of data, and their datasets continue to expand as they drive their autonomous vehicles around.

That being said, when it comes to sign detection alone, it has been found that doing a hough transformation along with some color detection is sufficient for many different signs. Stop signs especially have a unique octagon shape along with a distinct red color that make them easier to identify by simply looking for their shape. Not to mention, convolutional neural networks have been found to have some issues with tampering (https://www.digitaltrends.com/cars/self-driving-cars-confuse-stickers-signs/)


## 4) Are you planning on re-implementing an existing solution, or propose your own new approach?
We are planning on re-implementing an existing solution. Our solution will mostly be based off of [this paper](https://ieeexplore.ieee.org/document/5716193) about sign detection. Our plan is as follows:

1) Create a binary image where only an acceptable range of red colors will be ones, and all other values will be zero. This acceptable range will be found through trial and error with some stop signs in different lightings, and general wear.
2) Next, we will experiment with different methods of edge detection. We will see what edge detector yields the best results.
3) Once we have an edge detector, we will use a hough transformation to find lines in the image. We will then use these lines to outline the red shapes.
4) We will use these outlines to check and see if there are any octagons (which we will assume to be stop signs as there are not many naturally occurring red octagons outside of stop signs) This step will also be the most difficult as lines detected by the hough transformation may not be complete, overlap perfectly, or be a slightly wrong shape depending on the perspective. 
5) Finally, we will attempt to apply our detection method to a live video and see if the detection can run fast enough and give us a warning when there is a stop sign spotted in the video. Of course, this should be no more complicating than simply applying our detection to every frame of the video. 


## 5) If you are proposing your own approach, why do you think existing approaches cannot adequately solve this problem? Why do you think you solution will work better?
Although we are not explicitly proposing our own solution, we are attempting to make as simple of an algorithm as possible in order to make the stop sign detection as fast as possible. In addition, we are doing so with limited resources as far as datasets go, so we need a method that does not require vast amounts of training and test images. This simple solution may result in some more inaccuracies, but the speed will hopefully make it more viable in a real-time situation. This would allow the occasional false positive/negative to get weeded out as we can just pay attention to stop signs that are recognized over time in a video.

## 6) How will you evaluate the performance of your solution? What results and comparisons are you eventually planning to show? Include a time-line that you would like to follow. 
* The performance of our solution is going to be dependent upon the response time in detecting a stop sign.
* The comparisons that contribute towards our solution are analyzing images containg stop signs against non-stop signs.
* After analyzing different signs, our result approach is going to be of displaying a “green” GO sign or a “red” STOP sign if there is a stop sign in the image.


| Date                        | Goal/Task                                                                                                                                                                       |
|-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 11/03                        | Reading input of multiple images (stop sign vs. other signs) and using color to output a binary image display (nearly complete already)                                                                                                                                                      |
| 11/10                        | Using different edge detection methods to detect shape of road sign to pick out the most efficient method |
| 11/15                       | Using hough transformation to detect lines on the image                            |
| 11/17 | Detecting a match between hough transform image and a standard stop sign image                                                                                                                                       |
| 11/20 | Applying our sign detection approach to a live video                                                                                                                                       |
| 11/25 | Displaying results (was a stop sign detected or not)                                                                                                                                       |
| 12/01 | Testing with enough photos to ensure high efficiency results                                                                                                                                       |
| 12/03 | Have completed website and presentation                                                                                                                                       |
