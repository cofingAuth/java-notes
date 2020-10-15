### [官网](https://docs.docker.com/)

### 简单整理
## docker 是什么？有什么作用？架构有什么？
1. docker 是一个开发员和系统管理员使用容器进行构建，运行，共享应用程序的平台
2. docker 作用
	1. 快速，一致地交付应用程序（生产与测试环境就如本地开发环境一样）
	2. 由于可移植性与轻量性，使可以动态管理工作负载并进行扩展或关闭应用程序和服务
	3. 同一硬件上可运行多个工作负载
3. docker 架构
	1. docker client 客户端，与守护程序通信
	2. docker daemon 守护程序，管理Docker对象（镜像、容器等），与其它守护程序通信
	3. docker registries 仓库(公共：Docker Hub 或者 私人)
## 容器和虚拟机
1. 容器：如运行在linux上，共享主机内核
2. 虚拟机：例如VM，虚拟机管理程序运行一个操作系统进行访问主机资源
## docker命令
1. docker stop ：停止运行中的容器
2. docker start ：启动一个停止的容器
3. docker search ： 公共仓库查镜像
4. docker rm ： 移除容器
5. docker rmi ： 移除镜像
6. docker pull 
7. docker push 
8. docker ps : 正运行的容器
9. docker exec ： 运行命令，常用：docker exec -it image /bin/bash/
10. docker images ： 查看所有镜像
11. docker builder

## Docker Compose
> 用于定义和运行多容器Docker应用程序的工具。通过Compose，您可以使用YAML文件来配置应用程序的服务。然后，使用一个命令，就可以从配置中创建并启动所有服务。

* The Compose file is a YAML file defining services, networks and volumes.
* 命令： 
	* docker-compose up ： 启动
	* docker-compose down ： 停止
	* docker-compose run ： 在服务上运行命令
	* docker-compose --help ： 查看帮助

## 建立镜像
#### Dockerfile
* 指导与建议  
	1. ephemeral containers 建短暂的容器："短暂"指可以停止并销毁容器，对其可重建和替换，使用最低的设置和配置
	2. build context 了解构建上下文：当前目录为构建上下文。默认情况下，假定Dockerfile在当前目录下，也可以使用-f指定其它位置。无论如何都会递归当前目录作为构建上下文发送到docker守护程序
	3. pipe Dockerfile through stdin 管道Dockerfile通过标准输入
	4. 用.dockerignore排除
	5. 使用多阶段构建
	6. 不用安装b不必要的软件包
	7. 解耦应用程序
	8. 最小化层数
	9. 排序多行参数
	10. 利用构建缓存
	11. FROM，LABEL，RUN，CMD，EXPOSE，ENV，ADD，COPY，ENTRYPOINT，VOLUME，USER，WORKDIR，ONBUILD

#### BuildKit