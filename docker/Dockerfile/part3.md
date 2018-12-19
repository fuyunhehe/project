# 目录

- [文件](#文件)
- [命令](#命令)

## 文件

`docker-compose.yml`用于定义 Docker 容器在生产中的行为方式

## 命令

- 概览

```shell
docker stack ls # 列出此 Docker 主机上所有正在运行的应用
docker stack deploy -c <composefile> <appname>  # 运行指定的 Compose 文件
docker stack services <appname> # 列出与应用关联的服务
docker stack ps <appname>   # 列出与应用关联的正在运行的容器
docker stack rm <appname>   # 清除应用
```

- 运行负载均衡

```shell
# 需要先运行以下命令，然后才能使用 docker stack deploy 命令
[thief@centos7-vm Dockerfile]$ docker swarm init
Swarm initialized: current node (vnzq5e0uh69cg55ndb9amdvam) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-44y2a0jptlqx7czelqzk4k5n8ochs2fpz1ta3q566je88rd03e-7z6ic3ek1uz4tmp6cyyvwa03k 192.168.1.47:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.

# 部署应用
# 后续可通过修改docker-compose.yml重新部署应用
# getstartedlab 为应用指定一个名称
[thief@centos7-vm Dockerfile]$ docker stack deploy -c docker-compose.yml getstartedlab
Creating network getstartedlab_webnet
Creating service getstartedlab_web

# 查看刚才启动的容器列表
[thief@centos7-vm Dockerfile]$ docker stack ps getstartedlab
ID                  NAME                  IMAGE                  NODE                DESIRED STATE       CURRENT STATE                ERROR               PORTS
ti63s6l0jas4        getstartedlab_web.1   fuyunhehe/test:hello   centos7-vm          Running             Running about a minute ago                       
e3wnescfflmb        getstartedlab_web.2   fuyunhehe/test:hello   centos7-vm          Running             Running about a minute ago                       

# 清除应用
[thief@centos7-vm Dockerfile]$ docker stack rm getstartedlab
Removing service getstartedlab_web
Removing network getstartedlab_webnet
```
