This is a document which contains the relevant theory concerning the DeepSORT algorithm, along with further links for further learning. This is a high-level overview and if you would like to understand further, we encourage you to go through the links - and then understand the files, classes and functions in the Tracking_DeepSORT/deep_sort folder one by one.

The deep sort algorithm is one that allows for multiple object tracking (MOT). Meaning, that if multiple objects belonging to the various classes that the object detector has been trained to detect are present in the frame - those objects can be identified, and tracked as being the same object from one frame to the next. This is done by understanding an objects motion and appearance features. 

However, here are some problems that are common to multiple object tracking on video streams / data.

1. Occlusion:
One of the most common challenges - imagine tracking the people in a busy market, very often a person is occluded from the camera since he is behind a group of people from the cameras perspective. In such cases, the object detector fails to detect the occluded person, but the tracker must ensure that the object is still tracked due to the information from the previous frame.
2. False positives and negatives - No object detector is 100 percent accurate. Care must be taken that these false detections don't hamper the tracker.
3. There are many more issues involved in tracking like variations in camera angles, non stationary cameras etc. 

Building-blocks of the DeepSORT algorithm. 
1. Kalman Filter:
This is a crucial step in DeepSORT. If Kalman Filters are new to you, please do check out the following playlist: https://www.youtube.com/playlist?list=PLn8PRpmsu08pzi6EMiYnR-076Mh-q3tWr. 