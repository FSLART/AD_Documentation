# ZED Bridge
The code can be found in [this repository](https://github.com/FSLART/ZED_Bridge).
## Objectives
- Use the ZED SDK to capture depth and color images;
- Gather the intrinsic parameters;
- Configure the extrinsic parameters;
- Publish the captured images and parameters.
## Inputs, outputs and TFs
### Inputs
- USB, I guess there's not much to say
### Outputs
Most of the topics are subscribed by the [mapper](../3.%20Mapper/README.md).
- `/zed/image_raw`: BGR8 image;
- `/zed/depth/image_raw`: Depth image in meters;
- `/zed/depth/camera_info`: Intrinsic parameters
- There are more but are kinda irrelevant...
### TFs
- `zed_camera_left->zed_camera_center`
- `zed_camera_right->zed_camera_center`
- `zed_camera_center->base_link`
## Theory
