autoware的map模块在perception/semantics目录下，包含以下三个包




# 1、Road Occupancy Processor

此包使用经过预处理的点云数据和ADAS地图数据生成指示道路状态的占用网格地图。

占用网格可以被概念化为一个一维的8位深度位图（a one dimensional 8-bit depth bitmap）。

此包发布了一个有四个不同的可能值的GridMap/OccupancyGrid:

- 未知区域
- 可行驶区域（free）
- 不可行驶区域（no road）
- 占据区

可以使用参数设置这些值

### 编译需要以下包

- GridMap (<http://wiki.ros.org/grid_map>)

### 先决条件

- 定位正常 (TF between velodyne and world)

  - PCD Map (map)
  - TF for the Map (tf map->world)
  - NDT matching (tf world-> base_link)
  - Base link to Localizer (tf base_link -> velodyne)

- 点云地面过滤方法发布在 /points_ground '和' /points_no_ground 话题上，Autoware 有以下三个可用算法:

  - `ray_ground_filter`
  - `ring_ground_filter`
  - `euclidean_cluster` with Planar Ground removal

- `wayarea2gridmap` 节点 from Semantics in the Computing Tab

  - Vector Map(也称作 ADAS Map) in

    ```
    /vector_map
    ```

    - VectorMap 必须定义道路区域 (way_areas)

### 数据订阅

This node subscribes to:

- 地面过滤 `/points_ground` (sensor_msgs::PointCloud)
- 地面上的障碍物点 `/points_no_ground` (sensor_msgs::PointCloud)
-  在 `/grid_map_wayarea` (grid_map::GridMap)节点中处理过的，包含道路信息的GridMap

### 数据发布

- `gridmap_road_status` 节点发布grid_map包的原始格式. 这可以用来扩展这个包的功能。
- `occupancy_road_status` 发布原始ROS message `nav_msgs::OccupancyGrid` .

### 启动方式

`roslaunch road_occupancy_processor road_occupancy_processor.launch`

### 快速设置

**This is only a quick guide, each node must be properly configured**

1. 加载 PointCloud Map (Map tab)
2. 加载 带有可用道路区域的VectorMap (Map tab)
3. 加载map 和 world之间的TF变换 (Map tab)
4. 加载 base_link和Localizer之间的TF变换  (Setup tab)
5. 启动voxel_grid_filter (Sensing tab/Points Downsampler)
6. 启动ray_ground_filter (Sensing tab/Points Preprocessor)
7. 启动ndt_matching (Computing tab/Localization)
8. 初始化 NDT pose
9. 启动wayarea2grid (Computing tab/Localization)
10. 启动road_occupancy_processor (Computing tab/Localization)

### 参数配置

- `points_ground_src` (default=`"points_ground"`) 定义只包含地面点的PointCloud源topic。
- `points_no_ground_src` (default=`"points_no_ground"`) 定义只包含障碍点的PointCloud源topic。
- `wayarea_topic_src` (default=`grid_map_wayarea`) 定义包含grid_map_msgs:GridMap和道路区域的topic的名称。
- `wayarea_layer_name` (default=`"wayarea"`) 定义topic中的layer的名称
- `wayarea_topic_src` 包含道路区域.
- `output_layer_name` (default=`road_status`) defines name of the output layer in the published GridMap object. If running several instances, each vehicle one can publish a different layer and later add them.
- `road_unknown_value` (default=`"128"`) indicates the value to fill in the occupancy grid when a cell is **UNKNOWN**.
- `road_free_value` (default=`"75"`) indicates the value to fill in the occupancy grid when a cell is **FREE**. Should be a number between 0-255.
- `road_occupied_value` (default=`"0"`) indicates the value to fill in the occupancy grid when a cell is **OCCUPIED**. Should be a number between 0-255.
- `no_road_value` (default=`"255"`) indicates the value to fill in the occupancy grid when a cell is **NO ROAD**. Should be a number between 0-255.

### 坐标系

occupancy grid与来自' /grid_map_wayarea '节点输入的GridMap发布在相同的坐标系中。

### 使用默认值的颜色表示

黑色表示矢量图中没有被定义为NO ROAD的区域

深灰色表示被此节点探测为 UNKNOWN 的区域.

轻灰色表示此区域可行驶.

白色表示占据的区域.

![road_occupancy_processor](autoware高精地图源码梳理.assets/road_occupancy_processor.jpg)

# 2、The object_map Package

此包包含从激光雷达传感器和ADAS地图中提取信息的节点。它处理后的结果以gridmap (grid_map)和/或占用网格(nav_msgs)的形式发布。

## Nodes 

### laserscan2costmap

此节点使用2D laserScan消息生成占用网格。

#### Input topics

默认值:/scan (sensor_msgs::LaserScan)from vscan.

#### Output topics

`/ring_ogm` (nav_msgs::OccupancyGrid) 输出OccupancyGrid，取值范围在0-100.

##### How to launch

It can be launched as follows:

1. 通过单击“计算”选项卡中“Semantics”部分下的laserscan2costmap复选框来使用Runtime Manager 。
2. 终端执行: `roslaunch object_map laserscan2costmap.launch`.

##### Parameters available in roslaunch and rosrun

- `resolution`分辨率定义网格中单元格(cell)的等效值(以米为单位)。值越小，精度越高，越耗费内存和计算成本(默认值:0.1)。
- `cell_width`表示占用网格围绕点云原点的宽度(默认值:150)，单位为米。
- `cell_height` 表示占用网格围绕点云原点的高度(默认值:150)，单位为米。
- `offset_x`offset_x表示占用网格偏移中心的距离，向左(-)或向右(+)移动(默认值:25)。
- `offset_y` offset_y表示占用网格的中心偏移到后面(-)或前面(+)(默认值:0)的距离。
- `offset_z` offset_z表示占用网格的偏移单元格z轴中心距离，低于(-)或高于(+)(默认值:0)。
- `scan_topic` 源点云topic

------

### wayarea2grid

Autoware中使用的ADAS地图可能包含道路区域的定义，这对于计算一个区域是否是可行驶区域十分有用。

此节点从ADAS地图(VectorMap)读取数据，并提取道路区域的3D位置。它将它们投射到一个占用网格中，如果该区域是道路，则将其设置为128，如果不是，则设置为255。使用这些值是因为这个节点使用8位位图(grid_map)。

#### Input topics

VectorMap发布的`/vector_map` (vector_map_msgs::WayArea) 
`/tf`中获取vector map(grid_frame) 与 传感器(sensor_frame) 的坐标变换 

#### Output topics

`/grid_map_wayarea` (grid_map::GridMap) 是生成的最终的GridMap，其值范围为0-255。
`/occupancy_wayarea` (nav_msgs::OccupancyGrid)是最终的占用网格，其值范围为0-255。

##### How to launch

It can be launched as follows:

1. 通过单击“Computing ”选项卡中“Semantics”部分下的wayarea2grid复选框来使用 the Runtime Manager。
2. 终端运行: `roslaunch object_map wayarea2grid.launch`

##### Parameters available in roslaunch and rosrun

- `sensor_frame` 定义了车辆原点的坐标系(默认值:velodyne)。
- `grid_frame` grid_frame定义了地图的坐标系(默认值:map)。
- `grid_resolution` grid_resolution以米为单位定义网格中单元格的等效值。值越小，精度就越高，这是以牺牲内存和计算成本为代价的(默认值:0.2)。
- `grid_length_x`grid_length_x表示占用网格围绕点云原点的宽度(默认值:25)，单位为米。
- `grid_length_y`grid_length_y表示占用网格围绕点云原点的高度(默认值为0)，单位为米。
- `grid_position_x` grid_position_x表示占用网格的中心是否会移动这个距离，左(-)或右(+)(默认值:0)。
- `grid_position_y` grid_position_y表示占用网格的中心是否会被这个距离移动，是向后(-)还是向前(+)(默认值:0)。

### grid_map_filter

This node can combine sensor, map, and perception data as well as other OccupancyGrids. It generates OccupancyGrids with useful data for navigation purposes. It publishes three different layers. More details are provided in the output topics section below.

该节点可以组合传感器、地图、感知数据以及其他占用网格，它生成包含有用的导航数据的占用网格。发布了三个不同的层次。更多的细节将在下面的输出主题部分中提供。

#### Input topics

VectorMap发布的`/vector_map` (vector_map_msgs::WayArea) 
the vector map (grid_frame) 和 传感器 (sensor_frame)发布的`/tf` 变换 .

costmap_generator代价地图计算的`/semantics/costmap_generator/occupancy_grid` (nav_msgs::OccupancyGrid)状态

#### Output topics

`/filtered_grid_map` (grid_map::GridMap) 它包含3层，也以常规占用网格消息的形式发布。

`distance_transform` (nav_msgs::OccupancyGrid)对从/realtime_cost_map话题中获得的占用网格应用距离变换，允许我们获得围绕障碍物点云的概率梯度。
`dist_wayarea` (nav_msgs::OccupancyGrid)包含距离转换和道路数据的组合，如wayarea2grid节点所述。
`circle` (nav_msgs::OccupancyGrid)  在/realtime_cost_map周围绘制一个圆圈

输出主题按照grid_map包中描述的方式配置，配置文件位于包的config文件夹中。

##### How to launch

1. 使用Runtime Manager，在“Computing”选项卡中选择“Semantics" 点击 `grid_map_filter` 复选框.
2. 终端执行: `roslaunch object_map grid_map_filter.launch`.

##### Parameters available in roslaunch and rosrun

- `map_frame` 定义了实时生成的costmap的坐标系统(默认值:map).
- `map_topic` 定义了发布实时发布costmap的主题(默认值:/realtime_cost_map)。
- `dist_transform_distance` 定义计算距离转换的最大距离，单位为米(默认值:2.0)。
- `use_wayarea` 表示是否使用道路区域来过滤costmap(默认值:true)。
- `use_fill_circle` 是否生成circle layer(默认为true)
- `fill_circle_cost_threshold` 表示决定是否绘制圆的最小代价阈值(默认值:20)
- `circle_radius` 定义圆的半径, 单位为米 (default: 1.7).

# 3、The costmap_generator Package

## costmap_generator

此节点读取PointCloud和/或DetectedObjectArray，并创建一个占用网格和GridMap，其中VectorMap是可选的。 需要订阅至少一个PointCloud或DetectedObjectArray来生成costmap。

#### Input topics

`/points_no_ground` (sensor_msgs::PointCloud2) : 由ray_ground_filter或compare map filter发布，它包含地面点过滤去除。

`/prediction/moving_predictor/objects` (autoware_msgs::DetectedObjectArray): 来自naive_motion_predict预测的障碍物

`/vector_map`:由 VectorMap发布. 

`/tf`  获取vector map(map_frame) 与sensor(sensor_frame)之间的变换 .

#### Output topics

`/semantics/costmap` (grid_map::GridMap)输出costmap, 取值范围从0.0-1.0.

`/semantics/costmap_generator/occupancy_grid` (nav_msgs::OccupancyGrid)输出OccupancyGrid,取值0-100.

##### How to launch

1. 在Runtime Manager 上Computing 中的*Semantics* 模块，点击 `costmap_generator` 复选框
2. 终端执行: `roslaunch costmap_generator costmap_generator.launch`.

##### Parameters available in roslaunch and rosrun

- `use_objects` 是否使用`DetectedObjectArray` 障碍物检测 (default value: true).
- `use_points`是否使用点云 `PointCloud` (default value: true).
- `use_wayarea` 是否使用VectorMap中的 `Wayarea` 定义 (default value: true).
- `objects_input` 的topic名称为 `autoware_msgs::DetectedObjectArray` (default value: /prediction/moving_predictor/objects).
- `points_input` Input topic for sensor_msgs::PointCloud2 (default value: points_no_ground).
- `lidar_frame` 激光雷达坐标系.代价是基于坐标系来计算的 (default value: velodyne).
- `map_frame` VectorMap坐标系 (default value: map).
- `grid_min_value` gridmap最小代价值 (default value: 0.0).
- `grid_max_value` gridmap最大代价值(default value: 1.0).
- `grid_resolution` gridmap分辨率 (default value: 0.2).
- `grid_length_x` gridmap x 方向的尺度 (default value: 50).
- `grid_length_y` gridmap y方向的尺度(default value: 30).
- `grid_position_x` 坐标在x方向的偏移 (default value: 20).
- `grid_position_y` 坐标在y方向的偏移 (default value: 0).
- `maximum_lidar_height_thres` 点云数据在高度上的最大阈值 (default value: 0.3).
- `minimum_lidar_height_thres` 点点云数据在高度上的最小阈值(default value: -2.2).
- `expand_rectangle_size` 扩展目标对象矩形区域的值 (default value: 1).
- `size_of_expansion_kernel` Kernel size for blurring effect on object's costmap (default value: 9).