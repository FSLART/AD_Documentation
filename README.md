# LART Autonomous Driving Documentation
This is the LART team autonomous driving department documentation.
The goal of this is to cover to some level of detail each software module to ease integration of new members, debugging, presenting to judges and future reference.
## Index
- [**ZED Bridge**](./2.%20ZED%20Bridge/README.md): uses the ZED SDK to capture images, gather intrinsic parameters, configure extrinsic parameters (tfs), and publish to ROS topics;
- [**Mapper**](./3.%20Mapper/README.md): detects cones in color images and deprojects its pixels as 3D coordinates, based on depth values and camera parameters (intrinsic and extrinsic). Cones are returned in the car ego frame;
- [**Planner**](./4.%20Planner/README.md): plans a path between the detected cones in the ego frame;
- [**Control**](./5.%20Control/README.md): controls steering position and motor RPM. The path is tracked using pure pursuit and the longitudinal control uses a "Proportional Integral Derivative" (PID) controller.

