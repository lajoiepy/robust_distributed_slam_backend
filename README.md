# Setup with 3 robots
This setup is for 3 robots equipped with a TX2 and Intel Realsense D435.
It assumes that robot 0 has the IP address = `192.168.12.100`, robot 1 = `192.168.12.101` and robot 2 = `192.168.12.102`. 

## On the TX2s
### Launch the camera driver
```
tx2-docker run --net host --privileged -v /dev/bus/usb:/dev/bus/usb -it realsense
export ROS_IP=192.168.12.10X
export ROS_MASTER_URI=http://192.168.12.100:11311
roslaunch realsense2_camera multi_robot_slam_realsense_camera.launch robot_id:=X
```

### Launch the distributed optimization /home/nvidia/Desktop
```
tx2-docker run --net host --privileged -v /home/nvidia/Desktop:/root/rdpgo_ws/src/robust_distributed_slam_module/scripts/log -it multi_robot_slam 
export ROS_IP=192.168.12.10X
export ROS_MASTER_URI=http://192.168.12.100:11311
roslaunch robust_distributed_slam_module generic_robot_slam.launch local_robot_id:=X other_robot_id:=Y port:=2458X --screen
```

## On a computer with blabbermouth (possibly one of the TX2)
### Launch Blabbermouth
```
./blabbermouth -s 10000 1:tcp:1:192.168.12.100:24580 2:tcp:1:192.168.12.101:24581 3:tcp:1:192.168.12.102:24582
```

## On the TX2s
### Launch the multi robot separators detection
```
docker exec -it CONTAINER_ID /bin/bash
export ROS_IP=192.168.12.10X
export ROS_MASTER_URI=http://192.168.12.100:11311
roslaunch multi_robot_separators multi_robot_slam_example.launch local_robot_id:=X other_robot_id:=Y --screen
```