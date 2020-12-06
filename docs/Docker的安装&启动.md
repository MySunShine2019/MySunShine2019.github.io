## Docker运行要求

1、系统`CentOS`7 64`Bit`

2、系统内核版本3.10以上

## 检查系统内核

```shell
uname -r
```

![uname -r 查看系统内核](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201122230730336.png)

## 查看`ip`地址

* windows

```shell
ipconfig
```

* Linux

```shell
ip addr
ifconfig
```

系统内核不同,查看`ip`地址的命令也会有差异

## Docker的安装

### 安装步骤

> Docker可运行在`Windows`、`CentOs`、`MAC`等操作系统上,这里小编给大家展示的是将Docker安装在`CentOs`7上

0、切换成`root`用户。若不切换，执行后面命令前都需加上`sudo`命令,否则会提示权限不足

命令:

```shell
su root
```

1、更新yum

命令：

```shell
yum update
```

> 说明:更新yum源,耗时很长,耐心等待吧

在更新过程中遇到如下两种情况,全部按`y`，`y`表示`yes`

![更新过程中遇到的提示全部按y](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201122214913917.png)

![更新过程中遇到的提示全部按y](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201122222029029.png)

2、安装软件包

命令:

```shell
# 格式:yum install -y 软件包列表
# 注意:多个软件包间用空格分隔
yum install -y yum-utils device-mapper-persistent-data 
```

* `yum-utils` : 提供了`yum-config-manager`yum配置管理功能
* `device-mapper-persistent-data` & `lvm2` : 是`devicemapper`驱动映射依赖

3、设置`yum`源

> 为什么要设置yum源？
>
> 因为yum源默认是国外的,设置成国内的yum源有加速效果.类似配置Maven的私服

```shell
# 格式: yum-config-manager --add-repo yum源加速地址
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

> `yum`源加速地址有很多,我这里采用的是阿里的。

设置成功的截图:

![yum-config-manager --add-repo yum源加速地址](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201122225351885.png)

4、安装Docker

命令：

```shell
# yum install -y 软件包列表
yum install -y docker-ce
```

安装成功的截图:

![yum install -y 软件包名称](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201122230433664.png)

5、校验docker是否安装成功

命令:

```shell
# 查看docker的版本号
docker -v
```

出现如`↓`图字样，则表示`Docker`安装成功

![docker -v 查看docker的版本号](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201122230536638.png)

### 设置`ustc`镜像

`ustc`提供了各种软件镜像的加速服务,服务是公共的、免费的，比如`Linux`、`Docker`

#### 步骤

1、获取阿里云的Docker镜像加速器地址

网址:https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors

![获取阿里云的Docker镜像加速器地址](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201122224716118.png)



加速器地址的作用:提升获取Docker官方镜像的速度

2、创建目录`/etc/docker`

```shell
mkdir -p /etc/docker
```

3、通过Vim编辑器,创建并编辑`/etc/docker/daemon.json`文件

```shell
vim /etc/docker/daemon.json
```

4、`daemon.json`文件中配置阿里云的Docker镜像加速器

```json
{
  "registry-mirrors": ["https://xxxxxx.mirror.aliyuncs.com"]
}
```

> 说明:`xxxxxx`表示你自己的阿里云的Docker镜像加速器地址

## 启动Docker

```shell
# 启动docker服务
systemctl start docker 
# 查看docker状态
systemctl status docker
# 设置docker开启自启动 (enable是自启动,disable是不自启动)
systemctl enable docker 
```

未启动docker的状态

![查看Docker状态(systemctl status docker)](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201122231950218.png)

启动docker后的状态

![查看docker状态(systemctl status docker)](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201122232416993.png)

----

以上就完成了Docker的安装和启动，觉得小编写的不错的话，给小编点个赞哦，我们下期再见！

关注下方小编的公众号，更多精彩等着你哦

![](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201122233341377.png)