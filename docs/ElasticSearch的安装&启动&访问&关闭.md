## 安装&启动

1、上传`ElasticSearch`安装包

安装包:

![上传ElasticSearch安装包](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201123170116279.png)

下载链接:`https://pan.baidu.com/s/1oyS9WR1fCljgfluI-cLtrw`

提取码:`czax`

> 若链接失联,请在评论区留言

上传成功截图:

![上传成功截图](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201123170329463.png)

2、解压到`/opt`目录下

```shell
# 格式:tar -zxvf tar包名称 -C 解压目的地
# 注意:-C,C是大写
tar -zxf /root/elasticsearch-7.4.0-linux-x86_64.tar.gz -C /opt
```

解压成功截图:

![tar -xvfz tar包名称 -C 解压目的地](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201123172219090.png)

3、创建普通用户

由于安全问题,`Elasticsearch`不准许`root`用户直接运行,需要在root用户下创建新用户.

```shell
# 创建新用户
# 格式:useradd 用户名
useradd javaCoder
```

![useradd 用户名 创建新用户](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201123173330093.png)

4、为刚创建的新用户设置密码

```shell
# 为新用户设置密码
# 格式:passwd 用户名
passwd javaCoder
```

![passwd 用户名 为用户设置密码](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201123173358219.png)

5、为刚创建的新用户授予操作`elasticsearch-7.4.0`目录的所有权限

```shell
# 格式:chown -R 用户名:用户组 文件/目录
chown -R javaCoder:javaCoder /opt/elasticsearch-7.4.0
```

授予成功后的截图:

![chown -R 用户名:用户组 文件/目录](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201123173754689.png)

6、修改`elasticsearch.yml`核心配置文件

```shell
vim /opt/elasticsearch-7.4.0/config/elasticsearch.yml 
```

`elasticsearch.yml`配置文件内容，如下所示：

```shell
# ======================== Elasticsearch Configuration =========================
# elasticsearch集群名称,默认是elasticsearch,可自定义
cluster.name: MyFirstElasticsearchCluster
# 节点名称，默认值随机定,建议自定义以方便管理
node.name: node-1
# 设置为0.0.0.0准许外网访问,比如windows访问
network.host: 0.0.0.0
# 设置Elasticsearch的http访问端口
http.port: 9200
# 初始化新集群时采取此配置来进行选举
# 格式:cluster.initial_master_nodes: ["节点名称列表,多个结点之前采用‘,’逗号分隔"]
# 例:
cluster.initial_master_nodes: ["node-1"]
```

![修改elasticsearch.yml核心配置文件](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201123180005661.png)

7、切换成`root`用户修改`sysctl.conf`、`20-nproc.conf`、`limits.conf`配置文件

> 为什么要修改这三个配置文件呢?
>
> 答:因为新建的`javaCoder`用户最大可建文件数太小,最大虚拟内存太小

* 切换成`root`用户

```shell
su root
```

* 修改`limits.conf`配置文件，设置最大可建文件数

```shell
vim /etc/security/limits.conf
```

在`limits.conf`配置文件末尾加上以下内容:

```config
# 格式:用户名 soft nofile 65536
javaCoder nofile 65536
# 格式:用户名 hard nofile 65536
javaCoder hard nofile 65536
```

![在limits.conf文件末尾添加内容](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201123181246622.png)

* 修改`20-nproc.conf`配置文件

```shell
vim /ect/security/limits.d/20-nproc.conf
```

在`20-nproc.conf`文件末尾添加以下内容

```txt
# 格式:用户名 soft nofile 65536
javaCoder soft nofile 65536
# 格式:用户名 hard nofile 65536
javaCoder hard nofile 65536
# * 表示通配符，代表所有
* hard nproc 4096
```

![修改20-nproc.conf配置文件](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201123181935938.png)

* 修改`sysctl.conf`配置文件，设置最大虚拟内存

```shell
vim /etc/sysctl.conf
```

在`sysctl.conf`配置文件中添加以下内容:

```shell
vm.max_map_count=655360
```

![修改sysctl.conf配置文件设置最大的虚拟内存](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201123182654894.png)

* 重新加载`sysctl.conf`配置文件

```shell
sysctl -p
```

![重新加载sysctl.conf配置文件](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201123182750747.png)

8、切换到刚创建的新用户下并启动`Elasticsearch`

* 切换刚创建的新用户下

```shell
# 切换用户 ,格式: su 用户名
su javaCoder
```

![image-20201123183213422](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201123183213422.png)

> 若在切换用户的过程中，要求输入该用户的密码，输入在执行`passwd javaCoder`命令时输入的密码即可.

* 切换到`%ELASTICSEARCH_HOME%/bin`可执行目录下

```shell
# 切换目录 ,格式:cd 目录
cd /opt/elasticsearch-7.4.0/bin
```

* 启动`elasticsearch`服务

```shell
./elasticsearch
```

启动成功截图

![./elasticsearch 启动Elasticsearch](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201123183922541.png)

注意:若未切换用户，用的是`root`用户启动的`elasticSearch`，则会出现如下图错误

![未采用新创建的用户启动elasticSearch的后果](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201124085940896.png)

-----

## 访问

如何在`windos`上访问`Linux`上安装并启动了的`ElasticSearch`呢?

步骤:

1、访问`ElasticSearch`前,确保防火墙已关闭

检查防火墙状态

```shell
systemctl status firewalld
```

![systemctl status firewalld](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201123184710959.png)

> 我的防护墙是关闭了的，若你的防火墙状态和我的不一致，请执行下面命令，来关闭防火墙

关闭防火墙

```shell
systemctl stop firewalld
```

禁用防火墙,禁止开机自启

```shell
systemctl disable firewalld
```

2、打开浏览器,输入`http://虚拟机ip:9200/`

> 9200是`http`协议的访问端口，可在`elasticsearch.yml`核心配置文件中进行修改

如何查看自己的虚拟机`ip`？

```shell
ip addr 
ifconfig
```

系统内核不同,查看`ip`的命令也会有所差异

输入后，重点关注以下内容即可

![http://虚拟机ip:端口号](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201123185951727.png)

## 关闭

如何关闭`Elasticsearch`服务呢?

在启动`ElasticSearch`服务的窗口下,按下`Ctrl+C`即可关闭`ElasticSearch`服务.

![关闭ElasticSearch](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201123190146448.png)

关闭`ElasticSearch`服务后再次去访问`ElasticSearch`服务是访问不通的.

----

以上就完成了`ElasticSearch`的安装&启动&访问&关闭，觉得小编写的不错的话，给小编点个赞哦，我们下期再见！

关注下方小编的公众号，更多精彩等着你哦

![](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201122233341377.png)