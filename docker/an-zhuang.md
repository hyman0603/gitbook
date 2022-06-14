# 安装

### 在Cenots上安装

移除旧版本

```
 sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

安装必要的一些系统工具

```
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

添加软件源信息

```
sudo yum-config-manager --add-repo https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
sudo sed -i 's+download.docker.com+mirrors.aliyun.com/docker-ce+' /etc/yum.repos.d/docker-ce.repo
```

更新并安装Docker-CE

```
sudo yum makecache fast
 sudo yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

开启Docker服务

```
sudo service docker start
```

卸载docker-ce

```
sudo yum remove docker-ce docker-ce-cli containerd.io docker-compose-plugin

sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```

### 在Ubuntu上安装

卸载旧版本

```
 sudo apt-get remove docker docker-engine docker.io containerd runc
```

