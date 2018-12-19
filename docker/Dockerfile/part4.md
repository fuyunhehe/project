# 目录

- [文件](#文件)
- [命令](#命令)

## 文件

## 命令

- 概览

```shell
docker-machine create --driver virtualbox myvm1 # 创建 VM（Mac、Win7、Linux）
docker-machine create -d hyperv --hyperv-virtual-switch "myswitch" myvm1 # Win10
docker-machine env myvm1                # 查看有关节点的基本信息
docker-machine ssh myvm1 "docker node ls"         # 列出 swarm 中的节点
docker-machine ssh myvm1 "docker node inspect <node ID>"        # 检查节点
docker-machine ssh myvm1 "docker swarm join-token -q worker"   # 查看加入令牌
docker-machine ssh myvm1   # 打开与 VM 的 SSH 会话；输入“exit”以结束会话
docker-machine ssh myvm2 "docker swarm leave"  # 使工作节点退出 swarm
docker-machine ssh myvm1 "docker swarm leave -f" # 使主节点退出，终止 swarm
docker-machine start myvm1            # 启动当前未运行的 VM
docker-machine stop $(docker-machine ls -q)               # 停止所有正在运行的 VM
docker-machine rm $(docker-machine ls -q) # 删除所有 VM 及其磁盘镜像
docker-machine scp docker-compose.yml myvm1:~     # 将文件复制到节点的主目录
docker-machine ssh myvm1 "docker stack deploy -c <file> <app>"   # 部署应用
```

- 安装docker machine

```shell
curl -L https://github.com/docker/machine/releases/download/v0.12.1/docker-machine-`uname -s`-`uname -m` >/tmp/docker-machine &&
chmod +x /tmp/docker-machine &&
sudo cp /tmp/docker-machine /usr/local/bin/docker-machine
```

- 加入节点

```shell
# master
#   列出节点信息
[thief@centos7-vm Dockerfile]$ docker node ls
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS      ENGINE VERSION
vnzq5e0uh69cg55ndb9amdvam *   centos7-vm          Ready               Active              Leader              18.09.0-beta5
#   查看节点信息
#   节点id vnzq5e0uh69cg55ndb9amdvam
#   返回结果: "ManagerStatus": {"Leader": true,"Reachability": "reachable","Addr": "192.168.1.47:2377"}
docker node inspect vnzq5e0uh69cg55ndb9amdvam
#   查看加入token
#   返回结果: SWMTKN-1-44y2a0jptlqx7czelqzk4k5n8ochs2fpz1ta3q566je88rd03e-7z6ic3ek1uz4tmp6cyyvwa03k
docker swarm join-token -q worker

# slave加入master
# token 为 上一步获取到的
# ip:port 为 ManagerStatus->Addr
docker swarm join --token SWMTKN-1-44y2a0jptlqx7czelqzk4k5n8ochs2fpz1ta3q566je88rd03e-7z6ic3ek1uz4tmp6cyyvwa03k 192.168.1.47:2377
```
