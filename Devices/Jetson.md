# Jetson 
NVIDIA Orin NX 8GB Super

# &#x1F4BB; SYSTEM
1. 本系统为基于Ubuntu22.04(Kernel 5.15.148-tegra)的jetpack6.2
2. 内置二进制包安装版ROS humble，源码编译版ROS noetic（不保证稳定性）
3. jtop查看本机各项参数

    以下为jetpack6.2的默认参数
    > | Software | Version |
    > | --- | --- |
    > | python | 3.10.12 |
    > | CUDA | 12.6.68 |
    > | CUDNN | 9.3.0.75 |
    > | Tensorrt | 10.3.0.30 |
    > | CUDA Arch BIN | 8.7 |
    > | L4T | 36.4.4 |
    > | torch | 2.5.0 |
    <div style="background: yellow">
    <p>Eigen、PCL、Python、OpenCV等为ROS Humble自带</p>
    </div>
# &#x1F4C2; Documents
/home/lnut/dependencies: 存放必要依赖文件，配系统时疏忽，将Orbbec ROS2放在此文件夹中，无法调整，维持原状态，就此记录。
/home/lnut/Workspaces: 存放工作空间，以个人姓名建立独立子文件夹
/home/lnut/Workspaces/Lab_Workspace: 存放实验室工程，非实验室项目禁止修改，破坏工程环境与代码内容影响程序运行和系统稳定性。

# &#x1F3AF; Projects
1. ROS noetic: 本机为YOPO工程编译ROS noetic源码，并仅在YOPO上测试稳定性，其他工程未验证。源码在[ros_catkin_ws](/home/lnut/Workspaces/Lab_Workspace/ros_catkin_ws)路径下.
    >TODO: 测试ROS noetic稳定并制作apt仓库供实验室分发安装使用。
2. MID360：本机安装了[Livox-SDK2](/home/lnut/Workspaces/Lab_Workspace/Drivers/Livox-SDK2)，和基于ROS noetic的[livox_ros_driver2_ros1](/home/lnut/Workspaces/Lab_Workspace/Drivers/livox_ros_driver2_ros1)和基于ROS humble的[livox_ros_driver2](/home/lnut/Workspaces/Lab_Workspace/Drivers/livox_ros_driver2)
3. Orbbec 335L: 本机安装了Orbbec SDKv2，和基于ROS noetic的[OrbbecSDK_ROS1](/home/lnut/Workspaces/Lab_Workspace/Drivers/OrbbecSDK_ROS1)和基于ROS humble的[OrbbecSDK_ROS2](/home/lnut/dependencies/OrbbecSDK_ws)
4. FastLIO: 本机安装了基于MID360的FAST-LIO，基于ROS1版本的[FAST-LIO-ROS1](/home/lnut/Workspaces/Lab_Workspace/FAST_LIO_ROS1)和ROS2版本的[FAST-LIO](/home/lnut/Workspaces/Lab_Workspace/Fast_LIO)
5. Issac ROS： 本机安装了NVIDIA的Issac ROS平台，提供了NVIDIA针对CUDA设计的ROS pipeline和demo。具体文档见[NVIDIA Isaac ROS](https://nvidia-isaac-ros.github.io/)

---
# &#x2757; Notice
1. 本机不建议使用ROS noetic进行开发，本ROS1环境仅为老师YOPO部署而建立，未经过其他稳定性检验
2. 建议使用ROS Humble开发，使用Docker构建个人环境，在~/Workspaces路径下建立个人文件夹XXX_Workspace
3. 在Lab_Workspace文件夹下预置了FastLIO（ROS1和ROS2双版本），雷达和相机驱动，为实验室默认工程、环境与默认环境兼容的demo测试使用。
4. Jetson使用Docker作为开发环境使用CUDA时，需要使用NVIDIA定制L4T镜像，且镜像L4T版本与本机L4T版本属于同一大版本序列
    > L4T R35（Based Ubuntu20.04）系统只能使用L4T R35 镜像，L4T R36（Based Ubuntu22.04）系统只能使用L4T R36 镜像。</br>
    > 即,不能跨系统版本使用基于CUDA的docker镜像（NVIDIA Jetson GPU驱动与内核高度绑定，等待未来NVIDIA将jetson GPU与kernal、driver解耦）
5. 默认系统环境可以安装基于CUDA加速的OpenCV,Ceres等。OpenCV需要满足版本大于4.8，Ceres版本建议为2.0，（ROS Humble需要OpenCV>=4.8,Ceres 2.0 保留旧版本API,Ceres 2.1以上使用新API）。安装新OpenCV时需要重新编译cv_bridge。