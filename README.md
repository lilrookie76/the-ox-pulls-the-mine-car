# the-ox-pulls-the-mine-car
# 仿真竞速包

  - 适用于第十六届全国大学生智能车竞赛创意组-迅飞智慧餐厅-线上仿真初赛

  - 目前采用方案：
rf2o激光里程计+阿克曼转向模型 
定位包-amcl 导航包-move_base（包含本地规划器-teb_local_planner）

  - 相信你安装了full版本的ROS，运行本工程还需要在终端用apt-get安装一个包ros-melodic-navigation
  - 首先根据迅飞官方使用说明文档readme.pdf进行编译前操作，以及有一些debug说明可以参考
  - 在`src`文件夹所在目录下，输入```catkin_make ```以完成编译
  - 在/home/(用户名)/.bashrc 文档末尾加上：
    - source /（省略工作空间路径，可在当前路径右键打开终端用pwd查看路径）/devel/setup.bash
  - 如何导航？
    - Step 1 :`roslaunch gazebo_pkg race.launch`
    - Step 2 :在rviz中设定目标点（使用2D Nav Goal笔）

# 各文件夹概述

## 1. racecar_control
  - 将ackermann指令下发给车辆

## 2. gazebo_pkg
- `launch` 
  - `includes`存放启动基本功能的launch文件 

- `config`: 存放参数配置文件（调参文件都在里面）

- `map`: 存放建好的平面栅格地图，pgm是地图，yaml是地图配置文件
  
- `scripts`: 顾名思义，存放py脚本文件。
  - `cmd_vel_to_ackermann_drive.py`: Twist到ackermann的转换
  - `keyboard_teleop.py`: 键盘控制车辆运动（建图用）
  
- `worlds`: 存放gazebo中的三维环境地图

- `meshes`:车辆3D模型 

- `urdf`:车辆描述文件

## 3. rf2o_laser_odometry
  - rf2o激光雷达里程计的package。
  - **如果已经安装过rf2o，则将其删除。**


## 4. teb_local_planner
  - teb本地规划器的package。
  - **如果已经安装过teb本地规划器，则将其删除。**

## 4. Simulation_results
  - 按照储存每个阶段的仿真结果（录屏），用于对比。

## 5. ucar_plane
  - 官方给的配置文件，按官方说明设置需要，仿真中不起作用。


# Q&A

 ## debug常识？

  - 不管是launch报错还是catkin_make报错，特别是在没有对其有任何改动的情况下，很大可能是缺包。这个时候可以通过百度谷歌搜寻解决方案，如果是缺包也可以通过阅读error信息得知缺的包有关内容，并上网查询该包的安装教程。

 ## 报错：[rf2o] ERROR: Eigensolver couldn't find a solution. Pose is not updated
  - 如果激光扫描数据包含+/- Inf和/或NaNs，则无法计算odom协方差
  - rf2o的BUG，参考[修复方法](https://github.com/artivis/rf2o_laser_odometry/commit/ae010765627e0446930599e3376a45ecaea2b422) 。

 ## 什么是ackermann?
  - 搜索`阿克曼转向几何`，是一种车辆运动规划模型，了解相关运动学模型。它适用于车辆型机器人。但本车为麦克纳姆全向轮。
