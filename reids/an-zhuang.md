# 安装

### 在 Ubuntu/Debian 上安装 <a href="#install-on-ubuntudebian" id="install-on-ubuntudebian"></a>

#### 通过仓库源安装

```
curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/redis.list

sudo apt-get update
sudo apt-get install redis
```

#### 通过源码安装

1. ### <mark style="background-color:orange;">下载源文件</mark>:smile:<mark style="background-color:orange;"></mark> <a href="#downloading-the-source-files" id="downloading-the-source-files"></a>
2. ####

#### 1、下载源文件

```
wget https://download.redis.io/redis-stable.tar.gz
```

**2、编译 Redis**

```
tar -xzvf redis-stable.tar.gz
cd redis-stable
make
```

如果编译成功，你会在`src`目录中找到几个 Redis 二进制文件，包括：

* **redis-server** : Redis 服务器本身
* **redis-cli**是与 Redis 对话的命令行界面实用程序。

要安装这些二进制文件到`/usr/local/bin`，请运行：

```
make install
```

**3、Linux 内核配置**

```
vim /etc/sysctl.conf

添加
vm.overcommit_memory = 1

为确保 Linux 内核特性透明大页面不会影响 Redis 内存使用和延迟，请使用以下命令：
echo never > /sys/kernel/mm/transparent_hugepage/enabled
```

\
