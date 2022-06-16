# DeepSORT
repo containing code for tracking using DeepSORT (developed on Ubuntu 20.04 LTS)

Instructions:

1. Create a virtual environment using `Python 3.8`.
2. Next, within your environment, run the command `pip install cmake`. This is a dependency for the ONNX library. 
3. Use the `requirements.txt` file to install the required dependencies, by running 
`pip install -r requirements.txt`
4. Open the `object_tracker_1.py` file and run it. The code should run out of the box.
5. If you wish to use a different YOLO model, you can set it in during the yolo object creation: `yolo=YOLO_Fast(model = '...')`. This is found in line 19. 

