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

4、在不停机的情况下升级或重启 Redis 实例

请按照以下步骤避免停机。

* 将您的新 Redis 实例设置为当前 Redis 实例的副本。为此，您需要一台不同的服务器，或者一台具有足够 RAM 以保持两个 Redis 实例同时运行的服务器。
* 如果使用单台服务器，请确保副本在与主实例不同的端口上启动，否则副本无法启动。
* 等待复制初始同步完成。检查副本的日志文件。
* 使用[`INFO`](https://redis.io/commands/info)，确保主副本和副本具有相同数量的密钥。用于`redis-cli`检查副本是否按预期工作并正在回复您的命令。
* 允许使用`CONFIG SET slave-read-only no`.
* 将所有客户端配置为使用新实例（副本）。请注意，您可能希望使用该[`CLIENT PAUSE`](https://redis.io/commands/client-pause)命令来确保在切换期间没有客户端可以写入旧主控。
* 一旦您确认主服务器不再接收任何查询（您可以使用[MONITOR 命令](https://redis.io/commands/monitor)检查），使用该命令将副本选择为主服务器`REPLICAOF NO ONE`，然后关闭您的主服务器。

{% hint style="danger" %}
Redis Cluster 4.0 在集群总线协议层面与 Redis Cluster 3.2 不兼容，因此需要批量重启。但是，Redis 5 集群总线向后兼容 Redis 4
{% endhint %}

\
