# 1. Server configuration
## hardware
|               |          name          |
| :-----------: | :--------------------: |
|   ***CPU***   |  Intel Core i9 12900K  |
| ***Memory***  |          64GB          |
|   ***GPU***   | NVIDIA RTX 3080Ti 12GB |
| ***Storage*** |   1TB SSD + 2TB HDD    |
> 2TB HDD 挂载在 /media/lnut 下(/media/lnut/Shared)，作为双系统的共享数据盘
## software
|                          |             version             |
| :----------------------: | :-----------------------------: |
|          System          | Ubuntu22.04 (linux kernal 5.15) |
|           CUDA           |              12.6               |
|         TensorRT         |              10.3               |
|          Python          |              3.10               |
|           ROS            |         humble、noetic          |
|          docker          |             28.3.3              |
| nvidia-container-toolkit |             1.17.8              |
### terminal
本机安装双版本ROS,在新建终端时需要根据提示选择对应的ROS版本</br>
1. ~/.bashrc 中部分配置文件如下：
```bash
# 清除所有 ROS 相关环境变量
function clean_ros_env() {
    # 清除标准 ROS 变量
    unset ROS_DISTRO ROS_VERSION ROS_PACKAGE_PATH ROS_ETC_DIR
    unset ROS_MASTER_URI ROS_HOSTNAME ROS_IP

    # 清除 ROS2 特有变量
    unset ROS_DOMAIN_ID RMW_IMPLEMENTATION AMENT_PREFIX_PATH
    unset COLCON_PREFIX_PATH ROS_LOCALHOST_ONLY

    # 清除 CMAKE 相关变量
    unset CMAKE_PREFIX_PATH

    # 重置 PYTHONPATH
    export PYTHONPATH=$(echo $PYTHONPATH | tr ':' '\n' | grep -v '/opt/ros' | grep -v '/home/lnut/Workspaces/Lab_Workspace/ros_catkin_ws' | paste -sd: -)

    # 重置 PATH
    export PATH=$(echo $PATH | tr ':' '\n' | grep -v '/opt/ros' | grep -v '/home/lnut/Workspaces/Lab_Workspace/ros_catkin_ws' | grep -v '/opt/ros/humble' | paste -sd: -)

    # 重置 LD_LIBRARY_PATH
    export LD_LIBRARY_PATH=$(echo $LD_LIBRARY_PATH | tr ':' '\n' | grep -v '/opt/ros' | grep -v '/home/lnut/Workspaces/Lab_Workspace/ros_catkin_ws' | paste -sd: -)
}

# 清理环境
clean_ros_env

# 根据选择加载环境
if [ "$edition" -eq "1" ]; then
    echo "Loading ROS Noetic environment"
    source /opt/ros/noetic/setup.bash
    export ROS_MASTER_URI=http://localhost:11311
    export ROS_HOSTNAME=localhost
else
    echo "Loading ROS2 Humble environment"
    source /opt/ros/humble/setup.bash
    export ROS_DOMAIN_ID=42
    export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
fi
```
2.shell脚本中需要输入命令选择ROS版本
例程如下：
```shell
gnome-terminal --tab -- bash -c "echo '1' | cat -
                                    <command>
                                    exec bash"
```

------
# 2. Virtual Environments
## python environment
python使用virtualenv / venv创建虚拟环境，不建议使用conda，conda会抢占本机原生python环境，影响ros使用
> 使用virtualenv / venv 创建虚拟环境并链接本机的torch、tensorrt、rospy
>
> > python3 -m venv <name> --system-site-packages --symlinks
> > 在使用ros时需要修改setup.cfg文件，添加配置
> >
> > ```bash
> > # src/{node}/setup.cfg
> > [build_scripts]
> > executable = /usr/bin/env python30
> > ```
> > 激活虚拟环境和关闭虚拟环境
> > ```bash
> > source <name>/bin/activate
> > deactivate
> > ```

## docker environment
使用 docker 进行本机环境管理
1. docker 镜像的拉取
```bash
docker pull <image_name>:version
```
2. docker 容器构建
```bash
docker run -it \
	--privileged
	--name <container_name> \
	-v /tmp/.X11-unix:/tmp/.X11-unix \
	-v /path/to/your/workspaces:/home/<name>/workspace \
	-w /home/<name>/workspace \
	--env DISPLAY=$DISPLAY \
	--runtime nvidia \
	--gpus all \
	<images_name>:version /bin/bash
```
3. container使用
```bash
docker start <container_name>				# 启动container
docker stop <conatiner_name>				# 关闭container
docker restart <container_name>				# 重启container
docker exec -it <container_name> /bin/bash	# 以bash方式进入container
```
4. NVIDIA-container (amd64)</br>
	amd64镜像允许跨系统版本使用镜像，且不依赖cuda版本。即，可以在ubuntu22.04中运行任何版本的Ubuntu镜像和其支持的所有版本cuda、pytorch、tensorrt等
5. Jetson-container (Jetson专属)</br>
	jetson中镜像严格依赖l4t版本，不允许跨系统跨代使用镜像中的cuda
> https://github.com/dusty-nv/jetson-containers.git
```bash
jetson-containers run $(autotag l4t-pytorch)	# pull and build a l4t-pytorch image
jetson-containers build --name=my_container pytorch transformers ros:humble-desktop		# build an image with ROS2, pytorch and transformers
```

# Git
1. 实验室代码管理仓库</br>
	LNUT-AGCL : url https://github.com/orgs/LNUT-AGCL
2. git 代码上传，使用者使用git上传自己的修改，并附上个人ID和修改内容
```bash
git add .
git commit --author="name <email>" -m "提交信息"
```

# Projects
## YOPO
1. [Original Paper](https://ieeexplore.ieee.org/document/10528860)
2. [Offical Code](https://github.com/TJU-Aerial-Robotics/YOPO.git)
3. [local Code](file:///home/lnut/Documents/Lab_workspaces/YOPO_workspace/YOPO/README.md)
4. [How to use YOPO](file:///media/lnut/Shared/System/YOPO.md)