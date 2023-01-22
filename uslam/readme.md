docker run -it --rm --privileged --net=host -v /run/user/1000/gdm/Xauthority:/root/.Xauthority -v /tmp/.X11-unix/:/tmp/.X11-unix/ -v ~/prj/Ebv-uslam/dat/:/uslam_ws/src/rpg_ultimate_slam_open/data -v /dev/bus/usb:/dev/bus/usb -e DISPLAY --env=NVIDIA_DRIVER_CAPABILITIES=all --gpus all uslam
source uslam_ws/devel/setup.bash

roslaunch ze_vio_ceres ijrr17.launch bag_filename:=dynamic_6dof.bag
roslaunch ze_vio_ceres ijrr17_events_only.launch bag_filename:=dynamic_6dof.bag

## Driver ##
apt-get install software-properties-common
add-apt-repository ppa:ubuntu-toolchain-r/test
add-apt-repository ppa:inivation-ppa/inivation-bionic
apt update
apt-get install libcaer-dev

cd uslam_ws/src/
catkin build davis_ros_driver

## Test Driver ##

catkin build dvs_renderer
roslaunch dvs_renderer davis_color.launch
rosrun rqt_reconfigure rqt_reconfigure

## Live Demo ##

cp uslam_ws/src/rpg_ultimate_slam_open/data/davis_calib_swe.yml uslam_ws/src/rpg_ultimate_slam_open/calibration/davis_calib_swe.yaml

- 3 terminal aç.
  - roscore
  - rosrun davis_ros_driver davis_ros_driver
  - roslaunch ze_vio_ceres live_DAVIS240C.launch camera_name:=davis_calib_swe timeshift_cam_imu:=0.019081827388163747
  - roslaunch ze_vio_ceres live_DAVIS240C_events_only.launch camera_name:=davis_calib_swe timeshift_cam_imu:=0.019081827388163747