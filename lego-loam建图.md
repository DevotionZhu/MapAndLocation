#### 跑lego-loam

roslaunch lego_loam run.launch 

#### 录制loam生成的地图

```bash
rosbag record -o out /laser_cloud_surround
```



#### 播放数据包

`rosbag  play 2019-06-25-13-22-37.bag --clock --topic /vlp16_0/velodyne_points /trunk_info/imu`

#### 保存pcd文件

```bash
rosrun pcl_ros bag_to_pcd out.bag /laser_cloud_surround pcd
```

