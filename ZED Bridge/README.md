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
Most of the topics are subscribed by the [mapper](LART/Mapper/README.md).
- `/zed/image_raw`: BGR8 image;
- `/zed/depth/image_raw`: Depth image in meters (FP32);
- `/zed/depth/camera_info`: Intrinsic parameters
- There are more but are kinda irrelevant...
### Published TFs
- `zed_camera_left->zed_camera_center`
- `zed_camera_right->zed_camera_center`
- `zed_camera_center->base_link`
## Theory
StereoLabs, the manufacturer of the camera, provide a ROS bridge for the ZED cameras. However, some problems were faced when trying to use it regarding dependencies and things not working as expected.
StereoLabs also provide a SDK to access the cameras, and that SDK works fine. With that in mind, this package was created as a minimalist alternative to the original ROS bridge. The SDK was used to gather and publish the images as well as intrinsic parameters to ROS topics. Extrinsic parameters as provided as ROS TFs.
The SDK allows capture of the color of each individual lens. Relatively to depth, it allows capture of a depth image in some referential (probably the camera center), or aligned to the right lens. To have the color aligned to the depth, the used images are the color of the right lens and the depth aligned to that lens.
The transform between the lenses and the camera center are defined, as well as between the camera center to the vehicle base frame.
