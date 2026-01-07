# Nav2 Costmap_2d

costmap_2d 软件包负责构建环境的二维代价地图（costmap），该地图由多个关于环境的“数据层”（layers）组成。它可以通过地图服务器（map server）或局部滚动窗口（local rolling window）进行初始化，并通过传感器观测数据来更新各数据层。该软件包提供插件接口，允许将不同数据层融合到代价地图中，并最终根据机器人轮廓（footprint）所定义的膨胀半径对障碍物进行膨胀处理。Nav2 版本的 costmap_2d 软件包基本上是 ROS1 导航栈（navigation stack）中对应版本的直接 ROS2 移植，仅因 ROS2 的支持需求而做了少量必要的修改。

有关代价地图及其内置插件的更多参数说明，请参阅其 [配置指南页面](https://navigation.ros.org/configuration/packages/configuring-costmaps.html)。[教程](https://navigation.ros.org/tutorials/index.html) 和 [首次设置指南](https://navigation.ros.org/setup_guides/index.html) 也为使用 costmap_2d 软件包及其各层提供了有用的上下文信息。此外，还提供了一个 [教程](https://navigation.ros.org/plugin_tutorials/docs/writing_new_costmap2d_plugin.html)，详细说明如何创建自定义的代价地图插件。

当前已知并可用的规划器插件列表，请参见 [导航插件列表](https://navigation.ros.org/plugins/index.html)。

## 在 RVIZ 中可视化体素（voxels）：

- 确保在 `voxel_layer` 参数作用域内将 `publish_voxel_map` 设置为 `True`。
- 打开一个新终端并运行以下命令：
  ```bash
  ros2 run nav2_costmap_2d nav2_costmap_2d_markers voxel_grid:=/local_costmap/voxel_grid visualization_marker:=/my_marker
  ```
  此处可将 `my_marker` 替换为你希望用于发布标记（markers）的任意话题名称。

- 然后在 RVIZ 图形界面中添加 `my_marker` 显示项。

### 勘误与注意事项：

- 要在三维空间中查看这些标记，你需要在 RVIZ 图形界面中将视图（view）切换为三维模式（例如 Orbit 模式）。
- 目前由于 RVIZ 中存在某些 bug，你需要在 RVIZ 显示设置中将 `fixed_frame` 设为 `odom` 坐标系。
- 在 Gazebo 仿真环境中使用从 bag 文件回放的点云数据时，可能会遇到问题，因为仿真时钟时间可能跳回到较早的时间点。

## 代价地图滤波器（Costmap Filters）

### 概述

代价地图滤波器是一种基于代价地图层的工具，可用于将名为“滤波掩码”（filter-masks）的空间相关栅格特征应用到地图上。当机器人进入或离开滤波掩码所标记的区域时，这些特征会被插件算法用于填充代价地图，从而让机器人能够动态调整其轨迹、行为或速度。  
代价地图滤波器的应用示例包括：  
- **禁入/安全区域**（keep-out/safety zones）：机器人绝不会进入的区域；  
- **速度限制区域**（speed restriction areas）；  
- **工业和仓储场景中的优先通道**（preferred lanes）等。

有关该功能的设计理念、架构及工作原理的更多信息，请访问 Nav2 官方网站：https://navigation.ros.org。
