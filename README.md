# Setup with 3 robots
This setup is for 3 robots equipped with a TX2 and Intel Realsense D435.
It assumes that robot 0 has the IP address = `192.168.12.100`, robot 1 = `192.168.12.101` and robot 2 = `192.168.12.102`. 

## On the TX2s
### Launch the camera driver
```
tx2-docker run --net host --privileged -v /dev/bus/usb:/dev/bus/usb -it realsense-tx2 MASTER_URI ROBOT_ID camera
```

### Launch the distributed optimization /home/nvidia/Desktop
```
tx2-docker run --net host --privileged -v /home/nvidia/Desktop:/root/rdpgo_ws/src/robust_distributed_slam_module/scripts/log -it multi_robot_slam MASTER_URI ROBOT_ID NEXT_ROBOT_ID optimization
```

## On a computer with blabbermouth (possibly one of the TX2)
### Launch Blabbermouth
```
./blabbermouth -s 10000 1:tcp:1:192.168.12.100:24580 2:tcp:1:192.168.12.101:24581 3:tcp:1:192.168.12.102:24582
```

## On the TX2s
### Launch the multi robot separators detection
```
tx2-docker run --net host --privileged -v /home/nvidia/Desktop:/root/rdpgo_ws/src/robust_distributed_slam_module/scripts/log -it multi_robot_slam MASTER_URI ROBOT_ID NEXT_ROBOT_ID separators record(optional)
```

### Launch with bag
```
tx2-docker run --net host --privileged -v /home/nvidia/Desktop:/root/rdpgo_ws/src/robust_distributed_slam_module/scripts/log -v /media/usb:/media -it multi_robot_slam MASTER_URI ROBOT_ID NEXT_ROBOT_ID bag BAG_PATH RECORDED_ID 
```
