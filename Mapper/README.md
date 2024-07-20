# Mapper
The current implementation can be found in [this repository](https://github.com/FSLART/mapper_speedrun). This was a rushed implementation in Python not performance oriented and was supposed to be more thoroughly thought in the future. 
## Objectives
- Detect cones in color images
- Reconstruct cones in image coordinates to 3D coordinates
- Have a small computational overhead and be fast
## Inputs, Outputs and TFs
### Inputs
Most of the topics used are published by the [ZED Bridge](LART/ZED%20Bridge/README.md).
### Outputs
Most of the topics are subscribed by the [planner](LART/Planner/README.md).
### Publised TFs
None
## Theory
Some stereoscopic cameras, of which the ZED is an example, have built-in hardware and firmware to estimate depth. The [ZED Bridge](../ZED%20Bridge/README.md) publishes both the color and the depth image. In the depth image, each pixel encodes the depth in meters in a 32-bit floating point number.
This mapper implementation detects the cones on the color image using a convolutional neural network, DAMO-YOLO. The neural network predicts the cone bounding boxes and its classes (blue, yellow, orange, orange small, unknown/other). The midpoint of each bounding box is considered as the cone position. 
Using the depth value estimated by the camera and by knowing the intrinsic and extrinsic parameters of the camera, the cone positions can be projected in the lens frame.
An overall diagram can be visible [here](./Architecture/Mapper.canvas).
### 2D to 3D Coordinates
The formulas to calculate the 3-dimensional $x,y,z$ coordinates from the $u.v$ image coordinates and intrinsic parameters $f_x, f_y, c_x, c_y$ are described below.

$$z = \frac{v}{\sqrt{1+\left(\frac{c_x-u}{f_x} \right)^2+\left(\frac{c_y-v}{f_y}\right)^2}}$$

$$x = \frac{c_x-u}{f_x} \cdot z$$

$$y = \frac{c_y-v}{f_y} \cdot z$$
## Implementation Details
### Pre-processing
The only pre-processing used is converting the color space, in this case from BGR to RGB, and resizing the image to inference size. The inference size is the sized used for all images during training. The inference size used is 640x640 pixels.
Pre-processing was performed using OpenCV and Numpy.
### Inference
Inference is performed using the ONNX runtime.
ONNX was adopted because the industry is willing to adopt that format as the standard, allowing interoperability.
This software package, the mapper, was developed in Python and not C++, in contrast to the majority of the rest of the software. The main reason is that, at the time, the ONNX runtime C++ version does not support CUDA or TensorRT acceleration on ARM64 devices, of which the NVIDIA Jetson is an example.
