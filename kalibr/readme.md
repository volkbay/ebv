## ETH KALIBR ##
Github sayfası ve oradaki wikiye bak.

### Docker ###
docker build -t kalibr .
docker run -it -v /tmp/.X11-unix/:/tmp/.X11-unix:ro -v /run/user/1000/gdm/Xauthority:/root/.Xauthority:ro -v /home/vokbay/prj/Ebv-kalibr/dat/:/data:rw -e DISPLAY kalibr

### ROS ###
- export DISPLAY=:1 
- export BAG_DIR=/data/davis_bag_
- rosrun kalibr kalibr_bagcreater --folder /data/bag/ --output-bag $BAG_DIR/davis.bag
- rosrun kalibr kalibr_calibrate_cameras --target $BAG_DIR/apriltag.yaml --bag $BAG_DIR/davis.bag --topics /cam0/image_raw --models pinhole-equi
- rosrun kalibr kalibr_calibrate_imu_camera --target $BAG_DIR/apriltag.yaml --cam $BAG_DIR/davis-camchain.yaml --imu $BAG_DIR/imu.yaml --bag $BAG_DIR/davis.bag --verbose --show-extraction --bag-from-to 40 80 --timeoffset-padding 0.4 --max-iter 50