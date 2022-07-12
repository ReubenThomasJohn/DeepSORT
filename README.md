# DeepSORT
repo containing code for object detector using YOLOv5 and an obstacle tracker using DeepSORT.

For the perception MIY - there are two projects, and the second, and more advanced projects relies on the first project. That is, the first project - object detection using yolov5, is a crucial step in the second tracking project. Hence, make sure you understand the first project well.

Instructions:

1. Create a virtual environment using `Python 3.8`.
2. Next, within your environment, run the command `pip install cmake`. This is a dependency for the ONNX library. 
3. Use the `requirements.txt` file to install the required dependencies, by running 
`pip install -r requirements.txt`
4. You will now need to understand the object detector code, and play around with it using your own input data. Preferably, write down each line yourself, as this will aid in better understanding. All the relevant theory has been added as comments inside the code itself. The file can be found in ```Tracking_DeepSORT\deep_sort\YOLOV5.py```. 
5. 2. Next, take a look at the ```DeepSORT_Theory.md``` file. This contains all the theory with some links to understand tracking using DeepSORT.
6. Open the `object_tracker_1.py` file and run it, while being in the repo's root directory, i.e. python3 DeepSORT/object_tracker_1.py. The code should run out of the box.
7. If you wish to use a different YOLO model, you can set it in during the yolo object creation: `yolo=YOLO_Fast(model = '...')`. This is found in line 19. All you need to do is change the ONNX model. This is the advantage of writing modular code. 
8. You can use this base code to perform tracking on whatever problem you need it for. You will simple need to retrain your object detector to detect the class you are interested in, and if needed, tune some of the thresholding constants. 

