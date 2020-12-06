## 前言

大家好，我是`Java码农ing`的作者，此篇文章或多或少可能有些瑕疵，欢迎大家明确指出我的缺点，为此感激不尽，我必有错改之无错加勉，我也同时能够希望和大家一起学习。如果觉得小编这篇文章写的不错的话，希望大家能够将这篇文章分享给周围的小伙伴们,好东西要一起分享。

## 安装`Ubuntu 18.04 LTS`子系统

1、打开控制面板，点击程序和功能

![打开控制面板](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201129163611650.png)

2、点击启用或关闭Windows功能--> 开启适用于Linux的Window子系统&虚拟机平台

![2](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201129163743198.png)

3、开启开发者选项

![3](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201129163344767.png)

4、打开应用商店

![4](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201129162910757.png)

5、搜索`Ubuntu`

![5](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201129163006508.png)

选择`Ubantu 18.04 LTS` 版本

![3](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201129163246556.png)

先获取,我这里已经获取过，没有截图给大家展示

后安装

![6](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201129153614755.png)

6、安装完后，点击启动

![7](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201129163428046.png)

7、设置用户名和密码

![8](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201129153539399.png)

## 查看版本

```shell
lsb_release -a
```

![lsb_release -a](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201129165028209.png)

## 设置root用户密码

```shell
sudo passwd
```

## 子系统安装位置

默认：`C:\Users\yg\AppData\Local\Packages\CanonicalGroupLimited.UbuntuonWindows_79rhkp1fndgsc`

不同的`Ubuntu`版本路径稍有不同，但是都是`Canonical`这个开头的

![子系统安装位置](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201129164949227.png)

## 打开Linux 子系统

1、`CMD` 命令行窗口运行，使用命令`bash`

2、`PowerShell` 命令行窗口运行，使用命令`bash`

3、Win + R ，如何`bash`

![打开Linux 子系统](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201129011819226.png)

## Linux 中访问 Windows 目录

直接访问` /mnt/c`、`/mnt/d `其中`c `和`d `代表 Windows 的 盘符，无需进行额外的挂载操作。

示例:

```shell
cd /mnt/c
```

## 系统基础配置

### 切换数据源为国内的镜像

1、备份`/etc/apt/sources.list`文件

```shell
cp /etc/apt/sources.list /etc/apt/sources.list_bak
```

2、

编辑并修改`/etc/apt/sources.list`文件

```shell
sudo vim /etc/apt/sources.list
```

先将原来的注释掉，然后再添加以下内容:

参考[ 清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/)，我这里以`Ubuntu18.04 LTS`为例

```shell
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
```

3、更新配置（一定不要忘记）

```shell
sudo apt-get update
```

## 卸载 Linux 子系统

三种方式：

1、用 `lxrun /uninstall /full `命令

- `lxrun `是 Windows 上 Linux 子系统的管理工具

2、用`ubuntu /?`命令

3、获取完整软件名称&卸载

```shell
# 获取完整软件名称
Get-AppxPackage *ubuntu*
# 卸载，以:CanonicalGroupLimited.Ubuntu18.04onWindows为例
Get-AppxPackage CanonicalGroupLimited.Ubuntu18.04onWindows | Remove-AppxPackage
```

## 禁用 Linux 子系统

```shell
Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
```

## 迁移

1、下载[LxRunOffline](https://github.com/DDoSolitary/LxRunOffline/releases)

![1](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201129152200831.png)

2、解压

![2](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201129152232774.png)

3、打开`cmd`,切换目录

```shell
cd %LxRunOffline_Home%/
```

> `%LxRunOffline_Home%`为变量,表示你本地解压`LxRunOffline.exe`文件后所在的目录

4、查看安装了哪些子系统

```shelll
%LxRunOffline_Home%/LxRunOffline.exe list
```

示例:

![%LxRunOffline_Home%/LxRunOffline.exe list](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201129154427835.png)

5、关闭`wsl`

```shell
wsl --shutdown
```

![wsl --shutdown](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201129154234307.png)

6、迁移

```shell
%LxRunOffline_Home%/LxRunOffline.exe move -n 第4步结果 -d 保存目录
```

示例:

![%LxRunOffline_Home%/LxRunOffline.exe move -n 第4步结果 -d 保存目录](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201129154055770.png)

7、查看迁移后的子系统安装目录

```shell
%LxRunOffline_Home%/LxRunOffline.exe get-dir -n 第4步结果
```

示例:

![%LxRunOffline_Home%/LxRunOffline.exe get-dir -n 第4步结果](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201129154353525.png)

# 安装`ubuntu`的桌面环境

1、安装`xorg`

```shell
sudo apt-get install xorg
```

2、安装`xfce`

```shell
sudo apt-get install  xfce4
```

3、安装`xrdp`

作用:提供一个windows远程桌面的服务端，让windows主机可以通过常用的远程桌面工具连接到`linux`服务器上。

```shell
sudo apt-get install xrdp
```

4、配置环境,修改`xrdp`监听端口，避免`windows`和`linux`产生冲突.

```shell
sudo sed -i 's/port=3389/port=3390/g' /etc/xrdp/xrdp.ini
```

截图:

![sudo sed -i 's/port=3389/port=3390/g' /etc/xrdp/xrdp.ini](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201129161620966.png)

4、将`xfce4`会话环境写到默认的会话环境配置文件中去

```shell
sudo echo xfce4-session > ~/.xsession
```

5、重启`xrdp`服务

```shell
sudo service xrdp restart
```

![sudo service xrdp restart](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201129161717751.png)

6、查看`ip`地址

```shell
ifconfig
ip addr 
```

系统内核不同，查看`ip`的命令也会有所差异

![ifconfig](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201129161810924.png)

## 远程连接

1、打卡远程桌面连接工具

![1](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201129160306367.png)

2、设置`ip`和端口

![2](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201129161941509.png)

3、点击`是`

![3](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201129162112242.png)

4、输入在`Ubuntu`中设置的用户名和密码

![root&root](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201129162249629.png)

5、成功页面

![success](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201129162756927.png)

-----

以上就完成了`Windows`的`Ubuntu`的子系统安装&远程连接，觉得小编写的不错的话，给小编点个赞哦，我们下期再见！

关注下方小编的公众号，更多精彩等着你哦

![](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201122233341377.png)