#### jekyll+apache docker实例明细

#### 基础配置
* docker ubuntu18.04 镜像
* sources.list 国内中科大的源


### jekyll 的docker镜像
```
安装curl程序包
不要使用 自带的ruby版本（因为缺少依赖包的dev文件，安装第三方包的时候，很容易缺少头文件及其他的）。
使用rvm安装，rvm会使用requirements来安装系统的依赖包。
/bin/bash请使用 -l 参数，这个参数用使用登陆bash来运行命令，rvm会在此状态下加载

gem安装的时候，不要忘记 jekyll 的包

添加两个卷 /data  /var/www/html

ENTERPOINT 接收参数为数组，合理划分命令参数

详情参考 jekyll目录内的 Dockerfile 文件
```

然后build项目

```
docker build -t clu_wong/jekyll:v1

-t是打标签, 用户名/镜像名:版本号
用户名和版本号可以忽略，版本号默认为latest
```

run项目

```
docker run -v [blog_path]:/data --name clu_blog clu_wong/jekyll:v1

-v 绑定目录:卷目录 --name 给容器指定名称 镜像信息
此run主要是为了编译项目，所以处理完就容器结束了，但是容器还会继续保留

把blog的信息，编译成html到 /var/www/html（这个卷是全镜像通用的）
```

### apache2的docker镜像配置
```
安装 apache2（2.4.29） 的包和vim（方便查看信息）
添加卷 /var/www/html
指定工作目录 /var/www/html

设定环境变量，这些变量都是apache2工作时候使用的，详情看apache目录内的Dockerfile

添加相应的工作目录，并创建对应目录

声明对外的端口 EXPOSE 80

给apache2.conf 添加ServerName声明
> RUN /bin/bash -l -c "echo 'ServerName localhost' >> /etc/apache2/apache2.conf"

指定入口 ENTERPOINT [ "/usr/sbin/apache2" ]
CMD [ "-D", "FOREGROUND" ]
CMD命令的参数，会追加到ENTERPOINT的参数后边，然后执行，所以ENTERPOINT如果出错，后边就不执行了
```

然后编译项目

```
docker build -t clu_wong/apache2:v1

```

运行容器

```
调试镜像（先把ENTERPOINT入口、CMD命令注释掉编译）
docker run -it -P --volumes-from clu_blog clu_wong/apache2:v1 /bin/bash

-it 交互式的终端 -P引用EXPOSE的端口号
--volumes-from clu_blog 指定卷的来源（源于哪个container）

运行镜像
docker run -d -P --volumes-from clu_blog clu_wong/apache2:v1

-d是分离执行，相当于后台进程
```

docker ps查看镜像的端口映射，然后本地访问该端口号。