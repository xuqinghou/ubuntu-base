# ubuntu-base
docker base image for ubuntu(init jdk)

FROM ubuntu:latest
MAINTAINER xuqinghou

#install JDK and TOMCAT
ADD jdk-8u31-linux-x64.gz  /usr/local/
#可选
#ADD tomcat /usr/local/     #tomcat下为star-appserver，如果直接复制star-appserver，只会复制目录里边的内容，不会复制目录本身
#ADD timezone /etc/
#启动脚本，用于从镜像启动容器时调用执行，见下面的ENTRYPOINT
#ADD onStart.sh /usr/local/

ENV JAVA_HOME=/usr/local/jdk1.8.0_31 CLASSPATH=$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar PATH=$PATH:$JAVA_HOME/bin
RUN echo "JAVA_HOME=/usr/local/jdk1.8.0_31\nCALSSPATH=$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar\nPATH=$PATH:$JAVA_HOME/bin" >> /etc/profile

#start tomcat
#ENTRYPOINT ["/usr/local/onStart.sh"]










build context

build context，一个自定义的文件夹，里面放置Dockerfile和一些需要的文件。
比如我的：
Dockerfile...这个是必须的
sources.list...自己在官方社区找的ubuntu14.04的源
vimrc...安装好vim后用到的配置文件。我事先配置好的，都是些基础的配置。

.
├── baseimage
│   ├── Dockerfile
│   ├── README.md
│   ├── sources.list
│   └── vimrc


Dokerfile
制作image有两种方法：

1.从现有容器通过commit命令创建

  dockerfile中不方便的操作可以在容器中操作然后提交
  没有批量启动容器的需要
  自己学、习练习，不需要移植

2.利用Dockerfile构建

  方便，灵活，可移植
  适合部署大量的镜像和容器
  
  
Dockerfile基础
  '#'表示注释，一般Dockerfile第一行注释容器的基本信息和版本。
  Dockerfile以命令：参数为基本构建语句，命令全部大写，后面的参数视命令而定
  
#FROM，必须是第一个命令项，表示我的镜像是以哪个镜像为基础构建的
FROM ubuntu
#MAINTAINER，后面接构建这的姓名和邮箱，方便联系
MAINTAINER xuqinghou <xuqinghou@qq.com>
#LABEL，用键值对的方式来指定image的元数据
LABEL Description="it is used as a basic image for DuoHuoStudio and my study.I will update and install vim." Vendor="Basic image"
#ADD，在构建时向Docker daemon传递文件
ADD sources.list /etc/apt/
#RUN，接操作和命令sudo apt-get install -y vim等
ADD sources.list /etc/apt/ 
#CMD，构建成功的镜像第一次启动时默认启动的命令
#CMD只有1条，一般默认在Dockerfile的最后
#如果有多个CMD，只有最后一个起作用
#CMD会被docker run ..后面的命令覆盖
CMD ["/bin/bash"]
#ENV，设置环境变量
ENV REFRESHED_AT









