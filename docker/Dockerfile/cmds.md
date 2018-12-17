# 目录

- [文件](#文件)
- [命令](#命令)

## 文件

`Dockerfile`容器的配置文件(主要文件)
`app.py`提供服务的运行文件
`requirements.txt`依赖文件

# 命令

- 概览

```shell
docker build -t friendlyname .  # 使用此目录的 Dockerfile 创建镜像
docker run -p 4000:80 friendlyname  # 运行端口 4000 到 90 的“友好名称”映射
docker run -d -p 4000:80 friendlyname   # 内容相同，但在分离模式下
docker ps   # 查看所有正在运行的容器的列表
docker stop <hash>  # 平稳地停止指定的容器
docker ps -a    # 查看所有容器的列表，甚至包含未运行的容器
docker kill <hash>  # 强制关闭指定的容器
docker rm <hash>    # 从此机器中删除指定的容器
docker rm $(docker ps -a -q)    # 从此机器中删除所有容器
docker images -a    # 显示此机器上的所有镜像
docker rmi <imagename>  # 从此机器中删除指定的镜像
docker rmi $(docker images -q)  # 从此机器中删除所有镜像
docker login    # 使用您的 Docker 凭证登录此 CLI 会话
docker tag <image> username/repository:tag  # 标记 <image> 以上传到镜像库
docker push username/repository:tag # 将已标记的镜像上传到镜像库
docker run username/repository:tag  # 运行镜像库中的镜像
```

- 创建image

```shell
# 当前目录下创建image
docker build -t friendlyhello .

# 查看本地image
[thief@centos7-vm Dockerfile]$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
friendlyhello       latest              f4b5f3cd73c9        42 minutes ago      131MB
python              2.7-slim            0dc3d8d47241        4 weeks ago         120MB
hello-world         latest              4ab4c602aa5e        3 months ago        1.84kB
```

- 运行程序

```shell
# 运行程序
# -d demean
# -p 本地与容器的port映射
[thief@centos7-vm Dockerfile]$ docker run -d -p 4000:80 friendlyhello
7425e1b476e6f452cacb224e1692009b44a4ba4eb8ff66a0a11cc3b4ad72f30a

# 查看本地进程
[thief@centos7-vm Dockerfile]$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                  NAMES
7425e1b476e6        friendlyhello       "python app.py"     2 minutes ago       Up 2 minutes        0.0.0.0:4000->80/tcp   vigilant_cori

# 停止程序
[thief@centos7-vm Dockerfile]$ docker stop 7425e1b476e6
7425e1b476e6

# 再查看就已经没有运行中的进程了
[thief@centos7-vm Dockerfile]$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

- 共享镜像

```shell
# 登陆docker
[thief@centos7-vm Dockerfile]$ docker login
Username: fuyunhehe
Password: 

Login Succeeded

# 标记镜像
[thief@centos7-vm Dockerfile]$ docker tag image username/repository:tag

# 发布镜像
[thief@centos7-vm Dockerfile]$ docker push username/repository:tag

# 拉取镜像
[thief@centos7-vm Dockerfile]$ docker run -p 4000:80 username/repository:tag
```
