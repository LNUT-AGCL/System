# 1. Server configuration
## hardware
|  | name |
| :---: | :---: |
| ***CPU*** | Intel Core i9 12900K |
| ***Memory*** | 32GB |
| ***GPU*** | NVIDIA RTX 3080Ti 12GB |
| ***Storage*** | 1TB SSD + 2TB HDD |
> 2TB HDD 挂载在 /media/lnut 下，作为双系统的共享数据盘
## software
|  | version |
| :---: | :---: |
| System | Ubuntu22.04 (linux kernal 5.15) |
| CUDA | 12.6 |
| TensorRT | 10.3 |
| Python | 3.10 |
| ROS | humble |
| docker | 28.3.3 |
| nvidia-container-toolkit | 1.17.8 |

------
# 2. Virtual Environments
## python environment
python使用virtualenv创建虚拟环境，不建议使用conda，conda会抢占本机原生python环境，影响ros使用
> 使用virtualenv / env 创建虚拟环境并链接本机的torch、tensorrt、rospy
>
> > python3 -m venv <name> --system-site-packages --symlinks
> > 在使用ros时需要修改setup.cfg文件，添加配置
> >
> > ```shell
> > # src/{node}/setup.cfg
> > [build_scripts]
> > executable = /usr/bin/env python3
> > ```
> > 激活虚拟环境和关闭虚拟环境
> > ```shell
> > source <name>/bin/activate
> > deactivate
> > ```

## docker environment
使用 docker 进行本机环境管理
1. docker 镜像的拉取
```shell
docker pull <image_name>:version
```
2. docker 容器构建
```shell
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
```shell
docker start <container_name>				# 启动container
docker stop <conatiner_name>				# 关闭container
docker restart <container_name>				# 重启container
docker exec -it <container_name> /bin/bash	# 以bash方式进入container
```

# 3. Git
## TODO 创建实验室Git托管平台