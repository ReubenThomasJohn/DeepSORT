# DeepSORT
repo containing code for tracking using DeepSORT (developed on Ubuntu 20.04 LTS)

Instructions:

1. Use the `requirements.txt` file to install the required dependencies, by running 
`pip install -r requirements.txt`
2. Open the `object_tracker_1.py` file and run it. The code should run out of the box.
3. If you wish to use a different YOLO model, you can set it in during the yolo object creation: `yolo=YOLO_Fast(model = '...')`. This is found in line 19. 

Note: We also also provided for you a pruned and quantized YOLOv5s ONNX model. Use this model, and see how it's FPS compares to the standard YOLOv5s model.  