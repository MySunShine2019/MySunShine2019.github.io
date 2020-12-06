## 环境准备

1、为`ElasticSearch`内置的`JDK`配置`JAVA_HOME`&`PATH`环境变量

提示:若你的`ElasticSearch`还未安装,请参考我的`https://blog.csdn.net/qq_41473905/article/details/110005285`这篇博客文章

2、下载Maven并用Maven构建`IK`分词器的`jar`依赖

### 1、配置`JDK`的`JAVA_HOME`&`PATH`环境变量

1.1、 切换到root用户，只有root用户才有权改`/etc/profile`文件

```shell
su root
```

1.2、 编辑`/etc/profile`环境变量文件

```shell
vim /etc/profile
```

1.3、在`/etc/profile`文件末尾添加以下内容:

```properties
# JDK Environment
# 配置JAVA_HOME环境变量
# 格式:export JAVA_HOME=%ELASTICSEARCH_JDK_HOME%
export JAVA_HOME=/opt/elasticsearch-7.4.0/jdk
# 配置PATH环境变量
# 格式:export PATH=$PATH:${JAVA_HOME}/bin
export PATH=$PATH:${JAVA_HOME}/bin
```

添加完后,按下`esc`进入命令行模式.再按下`:`进入底行模式,再按下`wq!`进行强制保存并退出.

![JDK 环境变量(JAVA_HOME&PATH)](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201125211554143.png)

1.4、 重新加载`/etc/profile`环境变量配置文件

```shell
source /etc/profile
```

1.5、在任意目录下执行`java -version`命令。检测环境变量是否配的正确

```shell
java -version
```

![java -version 查看jdk版本](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201125211339409.png)

### 2、安装&配置`Maven`

2.1、上传`Maven`安装包

安装包：

![maven安装包](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201125205116395.png)

网盘链接:`https://pan.baidu.com/s/13I_AQaA9E6tQvX8t4PiYPw`

提取码:`gbez`

上传成功截图:

![上传成功的截图](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201125205054531.png)

2.2、解压到`/opt`目录下

解压命令:

```shell
# tar -zxvf tar.gz包名称 -C 解压目的地
# 例:
tar -xvfz /root/apache-maven-3.6.1-bin.tar.gz -C /opt
```

解压成功的截图:

![tar -xvfz tar.gz包命令 -C 解压的目的地](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201125205615758.png)

2.3、设置软链接

> 软连接就相当于Windows上的快捷方式

```shell
ln -s /opt/apache-maven-3.6.1 /opt/maven
```

设置成功后的截图:

![ln -s 源资源 软链接别名](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201125210032008.png)

2.4、设置`MAVAN_HOME`&`PATH`环境变量

首先,切换到`root`用户,只有root用户有权改`/etc/profile.d/maven.sh`文件

```shell
su root
```

其次，创建并编辑`/etc/profile.d/maven.sh`文件

```shell
vim /etc/profile.d/maven.sh
```

在`/etc/profile.d/maven.sh`配置文件中添加以下内容:

```shell
export MAVEN_HOME=/opt/maven
export PATH=${MAVEN_HOME}/bin:${PATH}
```

添加完后,按下`esc`进入命令行模式.再按下`:`进入底行模式,再按下`wq!`进行强制保存并退出.

![查看maven.sh配置文件的配置](http://ww1.sinaimg.cn/large/006pSjz9ly1gl1qoxrrg8j30i7040gls.jpg)

2.5、重新加载`maven.sh`配置文件,使得刚才的配置生效

```shell
source /etc/profile.d/maven.sh
```

2.6、在任意目录下执行`mvn -v` ，检测maven是否安装成功并且环境变量配的没问题

```shell
mvn -v
```

![mvn -v查看maven版本](http://ww1.sinaimg.cn/large/006pSjz9ly1gl1qnrfkrrj30tm053mxk.jpg)

## `IK`分词器安装

0、切换到`/root`目录下

```shell
cd /root
```

1、下载`IK`分词器安装包

```shell
wget https://github.com/medcl/elasticsearch-analysis-ik/archive/v7.4.0.zip
```

2、解压`IK`分词器到`/opt`目录下

由于`v7.4.0.zip`的`IK`分词器安装包是`zip`包,不是`gz`包,所以需用`unzip`解压,而不能使用`tar`

首先,安装`unzip`&`zip`,若你有安装过,请忽略此步

```shell
yum install zip
yum install unzip
```

其次,将`IK`分词器解压

```shell
# unzip解压
# 格式:unzip zip压缩包
unzip /root/v7.4.0.zip
```

![unzip ik分词器](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201125214539547.png)

最后,将解压后的`IK`分词器剪切到`/opt`目录下

```shell
mv /root/elasticsearch-analysis-ik-7.4.0 /opt
```

![mv source target](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201125214832306.png)

3、编译`jar`包

首先,切换到`elasticsearch-analysis-ik-7.4.0`目录下

```shell
cd /opt/elasticsearch-analysis-ik-7.4.0/
```

其次,执行打包指令

```shell
mvn package
```

打包成功的截图:

![mvn package](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201126095734944.png)

问题:

* 若打包失败,请检查一下`Maven`本地仓库的配置以及`Maven`私服的配置

![mvn package-->打包失败](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201125222539639.png)

回顾：

* 配置maven的本地仓库

```xml
<localRepository>/path/repository</localRepository>
```

![maven本地仓库的配置](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201125232418917.png)

* 配置maven的私服镜像

```xml
<mirror>
      <id>alimaven</id>
      <name>aliyun maven</name>
      <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
      <mirrorOf>central</mirrorOf>
</mirror>
```

![阿里云私服镜像](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201125232230498.png)

4、完成`elasticsearch-analysis-ik-7.4.0.zip`压缩包拷贝

说明:

`mvn package`命令执行完后，会生成`/opt/elasticsearch-analysis-ik-7.4.0/target/releases`目录,将该目录中的`elasticsearch-analysis-ik-7.4.0.zip`压缩包拷贝到`/opt/elasticsearch-7.4.0/plugins/analysis-ik`目录下,并进行解压。

![mvn package命令执行完后生成的zip压缩包](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201126100036520.png)

实现步骤:

首先,在`/opt/elasticsearch-7.4.0/plugins/`下创建`analysis-ik`目录

```shell
mkdir -p /opt/elasticsearch-7.4.0/plugins/analysis-ik
```

> -p,表示批量创建

创建成功的截图:

![mkdir -p 目录-->创建目录](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201125232631188.png)

其次,拷贝`/opt/elasticsearch-analysis-ik-7.4.0/target/releases/`目录下的`elasticsearch-analysis-ik-7.4.0.zip `压缩包到`/opt/elasticsearch-7.4.0/plugins/analysis-ik`目录下

```shell
cp -R /opt/elasticsearch-analysis-ik-7.4.0/target/releases/elasticsearch-analysis-ik-7.4.0.zip      /opt/elasticsearch-7.4.0/plugins/analysis-ik
```

拷贝成功的截图:

![cp -R source target-->拷贝操作](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201126100338854.png)

再其次,切换到`/opt/elasticsearch-7.4.0/plugins/analysis-ik`目录下

```shell
cd /opt/elasticsearch-7.4.0/plugins/analysis-ik
```

最后，解压`/opt/elasticsearch-7.4.0/plugins/analysis-ik`目录下的`elasticsearch-analysis-ik-7.4.0.zip`压缩包

```shell
unzip /opt/elasticsearch-7.4.0/plugins/analysis-ik/elasticsearch-analysis-ik-7.4.0.zip
```

解压成功截图:

![unzip source--->解压zip包](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201126100704064.png)

5、拷贝辞典

将`/opt/elasticsearch-analysis-ik-7.4.0/config/`目录下所有文件拷贝到`/opt/elasticsearch-7.4.0/config`的`config`目录中。

```shell
cp -R /opt/elasticsearch-analysis-ik-7/config/* /opt/elasticsearch-7.4.0/config
```

拷贝成功的截图:

![cp -R source target -->拷贝](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201126101510957.png)

6、为新用户重新授予操作`/opt/elasticsearch-7.4.0`所有权限

> 为什么要为新用户重新授予操作`/opt/elasticsearch-7.4.0`所有权限呢?
>
> 答:从上面张图可得知,我们将`/opt/elasticsearch-analysis-ik-7/config/*`目录中的所有文件拷贝到`/opt/elasticsearch-7.4.0/config`目录中后,所拷贝的这些文件都隶属于`root`用户,而`ElasticSearch`默认是不支持`root`用户启动的,则会导致在使用新用户启动`ElasticSearch`后,拷贝过来的这些文件并没有生效,`IK`分词器的功能也不能正常使用,所以需要重新授权。

授权命令:

```shell
# 格式:chown -R 用户名:用户组 source
chown -R javaCoder:javaCoder /opt/elasticsearch-7.4.0
```

> 我这里的新用户是`javaCoder`,你在操作的时候要根据实际情况来定

授完权后的效果:

![chown -R 用户名:用户组 source](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201126154819592.png)

7、重启`ElasticSearch`

首先，在启动`elasticsearch`服务的窗口下,按下`Ctrl + C`来关闭`elasticsearch`服务

其次,进入到`%ELASTICSEARCH_HOME%/bin`可执行目录下

```shell
cd /opt/elasticsearch-7.4.0/bin
```

再其次,切换成非`root`用户,因为`elasticsearch`默认不支持`root`用户启动

```shell
# 格式: su 用户名
su javaCoder
```

最后,启动`elasticsearch`服务

```shell
./elasticsearch
```

----

以上就完成了`ik`分词器的安装，觉得小编写的不错的话，给小编点个赞哦，我们下期再见！

关注下方小编的公众号，更多精彩等着你哦

![](https://cdn.jsdelivr.net/gh/MySunShine2019/imgbed/img/image-20201122233341377.png)

