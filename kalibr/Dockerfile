FROM osrf/ros:noetic-desktop-full

# Dependencies we use, catkin tools is very good build system
# https://github.com/ethz-asl/kalibr/wiki/installation
RUN apt-get update && DEBIAN_FRONTEND=noninteractive \
	apt-get install -y \
	git wget autoconf automake nano \
	python3-dev python3-pip python3-scipy python3-matplotlib \
	ipython3 python3-wxgtk4.0 python3-tk python3-igraph \
	libeigen3-dev libboost-all-dev libsuitesparse-dev \
	doxygen \
	libopencv-dev \
	libpoco-dev libtbb-dev libblas-dev liblapack-dev libv4l-dev \
	python3-catkin-tools python3-osrf-pycommon

# Create the workspace and build kalibr in it
ENV WORKSPACE /catkin_ws

RUN mkdir -p $WORKSPACE/src && \
	cd $WORKSPACE && \
	catkin init && \
	catkin config --extend /opt/ros/noetic && \
	catkin config --cmake-args -DCMAKE_BUILD_TYPE=Release
#	catkin config --merge-devel

# ADD . $WORKSPACE/src/kalibr
WORKDIR $WORKSPACE/src
RUN		git clone https://github.com/ethz-asl/kalibr.git

RUN 	mkdir -p $WORKSPACE/build/aslam_cv_python 
WORKDIR $WORKSPACE/build/aslam_cv_python
# RUN		catkin build --get-env aslam_cv_python | catkin env -si /usr/bin/make --jobserver-auth=3,4

WORKDIR $WORKSPACE
RUN		catkin build -j$(nproc)

RUN		echo "export KALIBR_MANUAL_FOCAL_LENGTH_INIT=1" >> /root/.bashrc
RUN		echo "source $WORKSPACE/devel/setup.bash" >> /root/.bashrc

ENTRYPOINT cd $WORKSPACE && /bin/bash