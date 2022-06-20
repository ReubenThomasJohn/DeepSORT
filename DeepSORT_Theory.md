# DeepSORT Theory:
This is a document which contains the relevant theory concerning the DeepSORT algorithm, along with further links for further learning. This is a high-level overview and if you would like to understand further, we encourage you to go through the links - and then understand the files, classes and functions in the Tracking_DeepSORT/deep_sort folder one by one.

The deep sort algorithm is one that allows for multiple object tracking (MOT). Meaning, that if multiple objects belonging to the various classes that the object detector has been trained to detect are present in the frame - those objects can be identified, and tracked as being the same object from one frame to the next. This is done by understanding an objects motion and appearance features. 

## Challenges in object-tracking:
However, here are some problems that are common to multiple object tracking on video streams / data.

1. Occlusion:
One of the most common challenges - imagine tracking the people in a busy market, very often a person is occluded from the camera since he is behind a group of people from the cameras perspective. In such cases, the object detector fails to detect the occluded person, but the tracker must ensure that the object is still tracked due to the information from the previous frame.
2. False positives and negatives - No object detector is 100 percent accurate. Care must be taken that these false detections don't hamper the tracker.
3. There are many more issues involved in tracking like variations in camera angles, non stationary cameras etc. 

## Building-blocks of the DeepSORT algorithm. 
1. Kalman Filter:
This is a crucial step in DeepSORT. If Kalman Filters are new to you, please do check out the following playlist: https://www.youtube.com/playlist?list=PLn8PRpmsu08pzi6EMiYnR-076Mh-q3tWr. 
In this particular implementation, the state contains 8 variables: (u, v, a, h, u', v', a', h'), where (u,v) are the boundin box centres, a is the aspect ratio, h is the height of the image. The others are the velocities of these 4 variables.
The Kalman filter assumes a simple linear velocity model, and helps in factoring the noise in detection. It uses the prior state to predict a good fit for the bounding boxes. Check the functions `kf.predict()` and `kf.update()` in the `KalmanFilter()` class.

2. Assignment:
Now that we have the tracked bounding boxes from the (i-1)th frame, we need a way to associate these boxes with the detections in the ith frame. This is known as the association problem. It arises, because the object detector, in our case `yolov5`, detects objects independently and does not know if the yellow car from the previous frame is the same yellow car in the current frame, or a different one.

This implementation uses 2 metrics to perform associations:
1. Distance metric: \
The squared mahalanobis distance is used. This metric is better tan the euclidean distance since we are measuring the distance between two normal distributions (from the Kalman Filter). Check out these links - \
a. https://stackoverflow.com/questions/48858104/squared-mahalanobis-distance-function-in-python-returning-array-why \
b. https://docs.scipy.org/doc/scipy/reference/generated/scipy.spatial.distance.mahalanobis.html \
c. https://www.machinelearningplus.com/statistics/mahalanobis-distance/ \
d. https://www.geeksforgeeks.org/how-to-calculate-mahalanobis-distance-in-python/
2. Hungarian algorithm:
This is a simple and efficient assignment algorithm. Look at this link to understand the hungarian algorithm. That creates a cost matrix, and then performs assignment. Also try researching why a simple coloum-wise max over all rows in the hungarian matrix cannot be used in real-time. 
a. https://www.geeksforgeeks.org/hungarian-algorithm-assignment-problem-set-1-introduction/
b. https://www.hungarianalgorithm.com/hungarianalgorithm.php
3. Appearance feature vector: \
This is the genius of the deepSORT algorithm. Essentially, we require a vector that can describe all the features fo a given image. In our case, we encode all the bounding boxes, and pass it into a classifier that is trained to detect all classes that we are concerned with. Therefore, the object detector, and this classifier must be able to detect the same classes. Once it is trained over the dataset, the final classification layer is removed, and the rest of the network is kept. This is usually a Dense layer having a single feature vector, that does the classification. 

Now, we pass the cropped images, containing only the detections (using the bounding boxes), and pass it into this this network to obtain the feature vector.

Thus, the updated metric will be:

`D = lambda * Dk + (1-lambda) * Da`. Here, Dk is the mahalonobis distance, Da is the cosine distance between the appearance feature vectors, and lambda is the weighting factor. 
The effect of Da is so vast, that the kalman filter can be turned off completely, that is, lambda = 0, and still acheive State of the Art (SOTA) results. \
cosine similarity help: \

a. https://www.delftstack.com/howto/python/cosine-similarity-between-lists-python/ \

b. https://www.geeksforgeeks.org/how-to-calculate-cosine-similarity-in-python/ 

## Custom feature-extractor: 
These days, simamese detectors with a triplet loss function are used for feature extraction. This is because Siamese networks work excellently on feature matching problems since they are trained to keep together data points of similar features while at the same time, learn to push away this groups far from other groups. More technically, the objective of the CNN loss function is to minimize the cosine distance between the feature vectors of the same class, and maximise the cosine distance between feature vectors of different classes. 
Here are a few helpful links: \
a. https://pyimagesearch.com/2020/11/30/siamese-networks-with-keras-tensorflow-and-deep-learning/ \
b. https://towardsdatascience.com/how-to-train-your-siamese-neural-network-4c6da3259463 
