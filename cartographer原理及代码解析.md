简单分工



机器人建模与仿真环境搭建

算法原理适配与编译运行

实验目的与现状、总结与分析

主流slam算法优缺点研究



其中，Cartographer_node是它的主要模块，它订阅了很多topic，其中/scan是激光雷达的数据，/imu是IMU的数据，我所做的主要工作就是参考Google的demo实现了Laser publisher和IMU publisher，并做了一些测试。



cartogapher 类似（（real-time correlative scan matcher）），引入了branch and bound的方式， 加快了闭环的查找。依据多分辨率多层的树型结构，单枝生长的方式(branch)，及时剪枝操作（bound），深度优先搜索确定闭环。 添加相应的闭环约束。构建优化问题，利用ceres优化。

这个过程中有一个很重要的过程是的scan和 submap的匹配，这里采用了BBS（Branch-and-bound scan matching）, 它可大幅提高精度和速度。



3. cartographer代码结构

common：定义了基本数据结构以及一些工具的使用接口。

sensor：定义了雷达数据及点云等相关的数据结构。

transform：定义了位姿的数据结构及其相关的转换。

kalman_filter: 主要通过kalman滤波器完成对IMU、里程计及基于雷达数据的估计位姿的融合，进而估计新进的laser scan的位姿。

mapping：定义了上层应用的调用接口以及局部submap构建和基于闭环检测的位姿优化等的接口。

mapping_2d和mapping_3d：对mapping接口的不同实现。

 

4. mapping_2d代码逻辑

4.1 cartographer::mapping_2d:: GlobalTrajectoryBuilder

cartographer::mapping_2d::GlobalTrajectoryBuilder类主要实现了接收处理上层应用传递的传感器数据的主要接口：

（1）      AddImuData用于接收处理上层应用传递的IMU数据。

（2）      AddOdometerPose用于接收处理上层应用传递的里程计数据。

（3）      AddHorizontalLaserFan用于接收处理上层应用传递的雷达数据。

其中包含重要的对象成员：

（1）      artographer::mapping_2d::LocalTrajectoryBuilder类的对象local_trajectory_builder_用于完成局部submap的构建。

（2）      cartographer::mapping_2d::SparsePoseGraph类的对象sparse_pose_graph_用于完成闭环检测及全局位姿优化。

在AddImuData和AddOdometerPose函数的实现中会将接收的相应传感器数据传递给local_trajectory_builder_对象处理。在AddHorizontalLaserFan函数的实现中则将新进的laser fan传递给local_trajector_builder_对象用于局部submap构建，如果该laser fan被成功插入到某个submap，那么该laser fan被插入后的相关信息则被传递给sparse_pose_graph_对象用于基于闭环检测的全局位姿优化。

 

4.2 cartographer::mapping_2d::LocalTrajectoryBuilder

cartographer::mapping_2d::LocalTrajectoryBuilder类主要完成局部submap的构建。其提供了接收处理传感器数据的public函数：

（1）      AddImuData用于处理IMU数据。

（2）      AddOdometerPose用于处理里程计数据。

（3）      AddHorizontalLaserFan用于处理雷达数据。

以及包含了一些重要的private成员：

（1）      ScanMatch成员函数基于submap已有的laser fan估计当前laser fan在submap中的位置。

（2）      cartographer::kalman_filter::PoseTracker类的对象
pose_tracker_用于融合基于雷达数据的laser fan的局部估计位姿、IMU数据以及里程计数据，进而估计出较优的laser fan的位姿。

在AddImuData和AddOdometerPose函数中会**将IMU数据和里程计数据传递给pose_tracker_进行处理。pose_tracker通过UKF不断融合IMU和里程计数据进而更新当前位姿，因此通过pose_tracker可以获取当前laser fan的估计位姿的一个较好的初始化值**。进一步的，在AddHorizontalLaserFan函数中会调用ScanMatch，ScanMatch函数中通过在submap中局部匹配得到的当前laser fan的估计位姿被pose_tracker_用来调整该laser fan的初始化值。这样pose_tracker_通过融合多传感器数据，进而能够估计出较优的laser fan的位姿。

 

4.3  cartographer::mapping_2d::SparsePoseGraph

cartographer::mapping_2d::SparsePoseGraph类主要完成基于闭环检测的全局位姿优化。其提供了接收处理新进被插入到submap的laser fan相关信息的public函数：

（1）      AddScan 对新进的laser fan进行闭环检测及在适当的时候进行全局优化。

以及一些重要的私有成员：

（1）      ComputeConstraintsForScan对新近laser fan信息进行处理并启动闭环检测scan match以及计算其约束，进而将约束添加到位姿优化目标中。

（2）      AddWorkItem将laser fan与ComputeConstraintsForScan绑定，并将任务加入到队列中。

（3）      HandleScanQueue依此调度队列中的任务。

（4）      sparse_pose_graph::ConstraintBuilder constraint_builder_ 用于完成laser fan的scan match以及约束计算。

（5）      RunOptimization优化目标。

在AddScan函数中会将laser fan相关信息与ComputeConstraintsForScan函数绑定，并将绑定好的任务通过AddWorkItem函数加入到队列中。HandleScanQueue函数则依次调度队列中的任务。第一次调用AddWorkItem时会直接启动ComputeConstraintsForScan任务，且在第一次ComputeConstraintsForScan任务时启动HandleScanQueue调度。在ComputeConstrainsScan中，通过constraint_builder_对象完成闭环检测的scan match以及约束计算。当所有约束计算完成时，则会进行RunOptimization优化目标。

 

4.4  Scan Match

LocalTrajectoryBuilder中的scan match策略与SparsePoseGraph中的scan match策略是不同的。前者使用scan_matching::RealTimeCorrelativeScanMatcher，后者则使用scan_matching::FastCorrelativeScanMatcher。二者的目标优化均是由scan_matching::CeresScanMatcher完成。

5. 总结

要将Cartographer的原理及实现详细地讲解清楚并不是短短两千字能完成的。为此，本文概述了Cartographer的理论及简要的梳理了cartographer源码的逻辑，目的在于起到抛砖引玉的作用，进而有利于选择性的研究相关理论及实现。

Cartographer的**重点内容是融合多传感器数据的局部submap创建以及用于闭环检测的scan match策略。重点内容对应的实现是：1）基于UKF的多传感器数据融合对应cartographer/kalman_filter目录下的文件；2）scan match策略对应cartographer/mapping_2d/scan_matching目录下的文件。后续有机会也会对这些重点内容及实现进行详细地梳理。**



![è¿éåå¾çæè¿°](https://img-blog.csdn.net/20161110144436386)

它的设计目标是实现低计算资源消耗，实时优化，不追求高精度，主要的贡献是提供了工程上高效率，它的一个关键点在于将branch and bound（分支限界算法）引入到闭环检测的过程中，提高与submap匹配的效率。

Scan是激光扫描的单帧数据，通过累加Scan来构建局部地图（Submap）。

Cartographer的实现并没有采用滤波方法，而是采用了类似图优化的模型进行位姿估计，我们以真实数据与观测方程之间的累计误差为目标函数，以机器人的位置作为优化变量，来求最优解。优化变量作为图的顶点，观测方程作为边。

 这样构造出来的很多很多submap是会产生累计误差的，最后通过闭环检测来消除这些误差，完成闭环。



闭环检测：识别去过过去场景的能力。

新来一张激光时，如何判断它以前是否在局部地图中出现过？有两种思路：一是根据我们估计的机器人位置，看是否与以前某个位置邻近；二是根据激光帧的外观，看它是否和以前关键帧相似。



**Cartographer主要理论是通过闭环检测来消除构图过程中产生的累积误差**。用于闭环检测的基本单元是局部地图。一个submap是由一定数量的激光帧构成。将一个laser scan插入其对应的submap时，会基于submap已有的laser scan及其它传感器数据估计其在该submap中的最佳位置。**submap的创建在短时间内的误差累积被认为是足够小的**。然而随着时间推移，越来越多的submap被创建后，submap间的误差累积则会越来越大。因此需要通过闭环检测适当的优化这些submap的位姿进而消除这些累积误差，**这就将问题转化成一个位姿优化问题**。

**当一个submap的构建完成时，也就是不会再有新的laser scan插入到该submap时，该submap就会加入到闭环检测中**。

闭环检测会考虑所有的已完成创建的submap。当一帧新的激光加入到地图中时，如果该帧的估计位姿与地图中某个submap的某个laser scan的位姿比较接近的话，那么**通过某种帧匹配策略就会找到该闭环**。

Cartographer中的帧匹配策略通过在新加入地图的激光帧的估计位姿附近取一个窗口，进而在该窗口内寻找该帧的一个可能的匹配，如果找到了一个足够好的匹配，则会将该匹配的闭环约束加入到位姿优化问题中。Cartographer的重点内容就是**融合多传感器数据的局部submap创建以及用于闭环检测的scan match策略的实现**。



算法代码包含两个部分：cartographer[2]和cartographer_ros[3]。cartographer主要负责处理来自雷达、IMU和里程计的数据并基于这些数据进行地图的构建，是cartographer理论的底层实现。**cartographer_ros则基于ros的通信机制获取传感器的数据并将它们转换成cartographer中定义的格式传递给cartographer处理**，与此同时也将cartographer的处理结果发布用于显示或保存，是基于cartographer的上层应用。

![è¿éåå¾çæè¿°](https://img-blog.csdn.net/20161110145019043)



cartographer功能包已经与ROS集成，但还没有提供二进制安装包，所以需要采用源码编译的方式进行安装。

我们需要将cartographer移植到自己的机器人上。cartographer提供了基于Revo LDS的demo，与我们使用的rplidar最相近，可以参考进行实现。



配置cartographer节点

​	cartographer的核心是cartographer_node节点，可以参考cartographer功能包中的demo_revo_lds.launch。在cartographer_ros功能包的launch文件夹下复制demo_revo_lds.launch重命名并修改以下代码。

修改lua文件

​	该文件的主要作用是进行参数配置。rplidar.lua文件主要需要设置坐标系与机器人匹配。通过Lua脚本配置参数的方法，每次修改参数后需要重新编译，否则参数无法生效。

​	此launch文件主要包含两部分的工作：一试运行cartographer节点，二是启动rviz可视化界面。当运行cartographer_node节点时，需要用一个由Lua编写的代码文件rplidar.lua，

