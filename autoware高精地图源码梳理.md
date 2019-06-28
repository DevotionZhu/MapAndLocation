# Road Occupancy Processor

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

