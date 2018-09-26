# Stop Sign Recognition

## Members

Albert Latham (latham2@wisc.edu) NetID: latham2

Azhar Siddiqui (siddiqui4@wisc.edu) NetID: siddiqui4

Kendel Chopp (kchopp@wisc.edu) NetID: kchopp

Guk Kim (gkim95@wisc.edu) NetID: gkim95
## Problem
The problem we are hoping to solve is that of stop sign recognition. This has many real world applications such as autonomous driving, driving performance monitoring, and others. We hope to create some sort of system which can take in an image (or video stream) and recognize whether or not there is a stop sign. Time permitting, we would also like to be able to check how far away stop signs are by using their relative size in the image, that way we can see how urgently we must stop.

Obviously there are other concerns in real world applications such as the line on the ground telling where to stop, other objects signaling a stop is necessary, and other traffic signals. However, identifying stop signs is a good first step which can be extrapolated to other objects as well.

## Steps to Solution
1) Get a dataset of images which include stop signs as well as no stop signs

2) Pick and train different 2-class classification neural networks and test their accuracy to come up with the best solution.

3) Recognize where the stop sign is in an image, and check how large it is.

4) Attempt to feed in a video stream and check to see if it can recognize a stop sign and the need to stop.

## Timetable
(In addition to weekly meetings on saturday to check in)

| Date                        | Goal/Task                                                                                                                                                                       |
|-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 9/26                        | Finish up project proposal                                                                                                                                                      |
| 10/6                        | Have a dataset of images (Pictures with stop signs, pictures with no stop sign/different types of signs, random images) Start looking into libraries (tensorflow, opencv, etc.) |
| 10/20                       | Have some preliminary objection recognition (Everyone create a neural network and help one another and then compare to see which is most successful)                            |
| 10/31 (Mid-term)            | Be able to identify whether or not there is a stop sign in an image                                                                                                             |
| 11/15                       | Catch up and complete stop sign recognition if incomplete, or add-on extra features (stop sign distance, etc.) Begin work on final presentation                                 |
| 12/1 (Presentation/Website) | Presentation & Website should be complete                                                                                                                                       |

## Potential Resources
Look into the following resources to potentially help with the object recognition

- Tensorflow (https://www.tensorflow.org/)
- OpenCV (https://opencv.org/)
- Keras (https://keras.io/)
- Open Detection (http://opendetection.com/)
- IEEE Trans. on Intel. Trans. (https://ieeexplore-ieee-org.ezproxy.library.wisc.edu/document/6352914)
- Benchmarking Traffic Sign Recognition Algos. (https://www-sciencedirect-com.ezproxy.library.wisc.edu/science/article/pii/S0893608012000457)
- Intl. Conf. on Intel. Sci. and Big Data Eng. (https://link-springer-com.ezproxy.library.wisc.edu/chapter/10.1007/978-3-319-23989-7_28)
