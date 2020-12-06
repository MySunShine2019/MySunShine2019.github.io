### 什么是Kibana？

答:Kibana是针对`ElasticSearch`的开源分析及可视化平台

KiBana有什么作用?

1、用来搜索、查看交互存储在ElasticSearch索引中的数据

2、可通过各种图表进行高级数据分析及展示

### 安装Kibana

1、上传`Kibana`安装包

安装包:

![Kibana安装包](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201124110613155.png)

网盘链接:`https://pan.baidu.com/s/1AgoxsutoOmqGm8UtjJe3Lg`

提取码:`a83f`

上传成功截图:

![上传Kibana成功的截图](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201124110912388.png)

2、解压`kibana`到`/opt`目录下

```shell
# 格式: tar -xvfz tar包名称 -C 目标目录
tar -zxvf /root/kibana-7.4.0-linux-x86_64.tar.gz -C /opt
```

解压成功的截图:

![解压成功后的截图](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201124111810535.png)

3、修改`kibana.yml`核心配置文件

```shell
vim /opt/kibana-7.4.0-linux-x86_64/config/kibana.yml
```

`kibana.yml`配置文件内容如下:

```shell
# kibana的http访问端口
server.port: 5601
# 指定ip地址,0.0.0.0表示支持远程访问,比如windows
server.host: "0.0.0.0"
# kibana服务名
server.name: "kibana-javaeden"
# elasticSearch主机地址
elasticsearch.hosts: ["http://192.168.200.128:9200"]
# 请求elasticSearch的超时时间,默认为30000,可自定义
elasticsearch.requestTimeout: 99999
```

![kibana.yml配置文件的配置](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201124151543467.png)

4、启动`kibana`

首先,切换到`%KIBANA_HOME%/bin`可执行目录下

```shell
# 格式: cd 目录
cd /opt/kibana-7.4.0-linux-x86_64/bin
```

![切换到可执行目录下](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201124151002571.png)

其次,启动`Kibana`

```shell
./kibana --allow-root
```

注意:`kibana`默认不支持`root`用户启动,若用`root`用户启动`kibana`,需要添加`--allow-root`参数,若未加入`--allow-root`会给出如下图的提示

![启动kibana ,未加入--allow-root参数](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201124183931335.png)

启动成功的截图:

![./kibana --allow-root 启动kibana](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201124175917700.png)

5、访问`Kibana`

打开浏览器,输入`http://虚拟机ip:5601/`访问

> 虚拟机ip为你自己的ip,5601是Kibana的端口号,端口号可在`kibana.yml`核心配置文件中进行修改

输入后,敲回车,看到如下结果，表示`Kibana`已安装成功

![核心面板介绍](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201124181222037.png)

6、如何关闭`Kibana`的服务呢?

在开启Kibana服务的窗口中,按下`Ctrl+C`即可关闭。

关闭成功的截图如下:

![关闭Kibana服务截图](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201124182015504.png)

> 关闭Kibana的服务后，想再次访问Kibana是访问不通的.

----

以上就完成了`Kibana`的安装&启动&访问&关闭，觉得小编写的不错的话，给小编点个赞哦，我们下期再见！

关注下方小编的公众号，更多精彩等着你哦

![](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201122233341377.png)