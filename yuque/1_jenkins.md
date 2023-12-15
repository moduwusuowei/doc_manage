# Jenkins 基础

## 背景 

实际开发中，代码在提交后会进行部署，然后由测试人员测试，一般的部署流程：

●提交代码

●问一下同组小伙伴有没有要提交的代码

●在服务器上对代码打包（war包，或者jar包）

●关闭正在运行的程序，重新启动新的jar包

●观察日志看是否启动成功

●如果有同事说，自己还有代码没有提交......再次重复上面步骤！！！！！（一上午没了）

可以看出过程非常繁琐而且很浪费时间，而 Jenkins 就是帮助我们自动打包部署

## Jenkins

### 介绍 

![img](https://cdn.nlark.com/yuque/0/2022/png/284379/1669440897225-e56b815b-09f9-4973-869e-df7d70a9594a.png?x-oss-process=image%2Fresize%2Cw_628%2Climit_0#averageHue=%23e4e0df&clientId=ubeceaa69-aa0e-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=293&id=u283c5036&margin=%5Bobject%20Object%5D&name=image.png&originHeight=366&originWidth=628&originalType=binary&ratio=1&rotation=0&showTitle=false&size=80900&status=done&style=none&taskId=u5421045b-011a-43aa-a864-7ee5dd6b5f6&title=&width=502.4#averageHue=%23e4e0df&crop=0&crop=0&crop=1&crop=1&id=UVaCb&originHeight=366&originWidth=628&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)



Jenkins：是一个开源的、基于Java的一种持续集成工具，用于监控持续重复的工作，使软件的持续集成变成可能

jenkins项目有两条发布线，分别是LTS长期支持版（或稳定版）和每周更新版（最新版）。建议选择LTS长期支持版，下载通用war包

官网下载地址：

[https://www.jenkins.io/download/](https://www.jenkins.io/zh/download/)

![image.png](1_jenkins.assets/image-1701330165756.png)



![image.png](1_jenkins.assets/image.png)



如果不想下载最新版本，也可以下载历史版本：https://get.jenkins.io/war-stable/

![image.png](1_jenkins.assets/image-1701330184602.png)

![image.png](1_jenkins.assets/image.png)



另外，jenkins官网下载的比较慢，在推荐一个清华大学的镜像网站：https://mirrors.tuna.tsinghua.edu.cn/jenkins/war-stable/

![image.png](1_jenkins.assets/image-1701330244949.png)

注意：下载后，就是个war包



### 1.环境准备 

#### 1.安装要求 

- 机器要求： 
  - 256 MB 内存，建议大于 512 MB
  - 10 GB 的硬盘空间（用于 Jenkins 和 Docker 镜像）

- 需要安装以下软件： 
  - JDK 11、Maven、Git

#### 2.安装Maven 

下载地址：

https://archive.apache.org/dist/maven/maven-3/3.6.3/binaries/

![image-20231130154710338](1_jenkins.assets/image-20231130154710338.png)



上传到 CentOS 7 虚拟机上上

![img](https://cdn.nlark.com/yuque/0/2022/png/284379/1669462781392-a4692000-536f-4c36-841b-75474d9bb8d8.png?x-oss-process=image%2Fresize%2Cw_680%2Climit_0#averageHue=%23012b48&clientId=u9335e2c7-86cb-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=103&id=u015a0b7d&margin=%5Bobject%20Object%5D&name=image.png&originHeight=129&originWidth=680&originalType=binary&ratio=1&rotation=0&showTitle=false&size=10942&status=done&style=none&taskId=u5caeea4f-5082-4b2a-9ac5-2d02426ccdf&title=&width=544#averageHue=%23012b48&crop=0&crop=0&crop=1&crop=1&id=OpUHD&originHeight=129&originWidth=680&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)



执行：

tar -zxvf apache-maven-3.6.3-bin.tar.gz



![img](https://cdn.nlark.com/yuque/0/2022/png/284379/1667734210348-2ebdd51c-03d6-45f5-a16f-8b12c99c29c0.png?x-oss-process=image%2Fresize%2Cw_727%2Climit_0#averageHue=%23012c49&clientId=u91e59db3-6ef3-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=128&id=ubf77e252&margin=%5Bobject%20Object%5D&name=image.png&originHeight=160&originWidth=727&originalType=binary&ratio=1&rotation=0&showTitle=false&size=15379&status=done&style=none&taskId=u44c60b18-a54f-4e21-a5b2-a4707a92c9a&title=&width=581.6#averageHue=%23012c49&crop=0&crop=0&crop=1&crop=1&id=EJryU&originHeight=160&originWidth=727&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)



然后，修改 setting.xml，配置阿里云仓库

```
<mirror>
  <id>nexus-aliyun</id>
  <name>nexus-aliyun</name>
  <url>http://maven.aliyun.com/nexus/content/groups/public</url>
  <mirrorOf>central</mirrorOf>
</mirror>
```

结果：

![image-20231130154744090](1_jenkins.assets/image-20231130154744090.png)



本地仓库：/home/soft/repo

![image-20231130154755167](1_jenkins.assets/image-20231130154755167.png)

#### 3.安装Git 

执行：

yum install git

![image-20231130154818488](1_jenkins.assets/image-20231130154818488.png)

 

### 安装、配置jenkins 

就是一个war包，可以直接运行

![img](https://cdn.nlark.com/yuque/0/2022/png/284379/1667734508024-e9db8329-0a82-4caf-8438-cbf00dc69917.png?x-oss-process=image%2Fresize%2Cw_711%2Climit_0#averageHue=%23012b49&clientId=u91e59db3-6ef3-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=135&id=u38d3e76c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=169&originWidth=711&originalType=binary&ratio=1&rotation=0&showTitle=false&size=15727&status=done&style=none&taskId=uc0157af0-81a9-4bda-951e-b4dec422f17&title=&width=568.8#averageHue=%23012b49&crop=0&crop=0&crop=1&crop=1&id=hi6Jy&originHeight=169&originWidth=711&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

执行：

java -jar jenkins.war



![image-20231130154953096](1_jenkins.assets/image-20231130154953096.png)



注意：启动jenkins时候，可能会报以下错误

![image-20231130155027708](1_jenkins.assets/image-20231130155027708.png)



针对于这个问题，只需要在linux中安装一个软件就可以了，比如：

![image-20231130155123645](1_jenkins.assets/image-20231130155123645.png)



之后在启动就没有问题了

然后浏览器访问：

http://192.168.228.122:8080/

![image-20231130155147766](1_jenkins.assets/image-20231130155147766.png)



![image-20231130155211659](1_jenkins.assets/image-20231130155211659.png)









设置用户密码

![image-20231130155259704](1_jenkins.assets/image-20231130155259704.png)



配置url，直接点击完成

![image-20231130155321834](1_jenkins.assets/image-20231130155321834.png)





接下来，安装maven插件

![image-20231130155415970](1_jenkins.assets/image-20231130155415970.png)



![image-20231130155426346](1_jenkins.assets/image-20231130155426346.png)



结果：

![image-20231130155447056](1_jenkins.assets/image-20231130155447056.png)



### 3.快速体验 

#### 3.1 创建构建任务 

![image-20231130155613266](1_jenkins.assets/image-20231130155613266.png)





1任务名称、任务类型

![image-20231130155632219](1_jenkins.assets/image-20231130155632219.png)



#### 3.2描述信息

![image-20231130155646875](1_jenkins.assets/image-20231130155646875.png)

#### 3.3源码管理：配置 git地址、代码分支

![img](1_jenkins.assets/image-1701329885568.png)

![image-20231130155723062](1_jenkins.assets/image-20231130155723062.png)

#### 3.4选择构建触发器

![image-20231130155805253](1_jenkins.assets/image-20231130155805253.png)

#### 3.5构建环境

![image-20231130155823139](1_jenkins.assets/image-20231130155823139.png)

#### 3.6构建前执行

![image-20231130155833823](1_jenkins.assets/image-20231130155833823.png)

#### 3.7Build：配置Maven

![image-20231130155854703](1_jenkins.assets/image-20231130155854703.png)



在页面的最下方可以：新增Maven

![image-20231130155917995](1_jenkins.assets/image-20231130155917995.png)



点击新增后：

![image-20231130155930527](1_jenkins.assets/image-20231130155930527.png)

指定pom.xml位置

![image-20231130155956413](1_jenkins.assets/image-20231130155956413.png)

#### 3.8构建后运行



![image-20231130160014331](1_jenkins.assets/image-20231130160014331.png)

 2. 运行构建任务 



回到首页，可以运行构建任务

![image-20231130160135113](1_jenkins.assets/image-20231130160135113.png)

查看状态：

![image-20231130160328138](1_jenkins.assets/image-20231130160328138.png)任务进度：

![image-20231130160412761](1_jenkins.assets/image-20231130160412761.png)



可以看到构建日志：

![img](https://cdn.nlark.com/yuque/0/2022/png/284379/1667789254684-f11e951b-2115-40c0-b188-79a010f3724b.png?x-oss-process=image%2Fresize%2Cw_750%2Climit_0#averageHue=%23f2f0f0&clientId=u64de5965-e773-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=479&id=u2fb51645&margin=%5Bobject%20Object%5D&name=image.png&originHeight=599&originWidth=1241&originalType=binary&ratio=1&rotation=0&showTitle=false&size=91172&status=done&style=none&taskId=u5b8e7a97-e73b-4caf-87b7-62f9fc13720&title=&width=992.8#averageHue=%23f2f0f0&crop=0&crop=0&crop=1&crop=1&id=slmXb&originHeight=599&originWidth=1241&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)



结果：

![img](https://cdn.nlark.com/yuque/0/2022/png/284379/1667789759849-44dfd204-6ac8-45d3-810e-a057f88d9704.png?x-oss-process=image%2Fresize%2Cw_750%2Climit_0#averageHue=%23edeaea&clientId=u64de5965-e773-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=280&id=ucfc75ce8&margin=%5Bobject%20Object%5D&name=image.png&originHeight=350&originWidth=1342&originalType=binary&ratio=1&rotation=0&showTitle=false&size=35648&status=done&style=none&taskId=uc1b0caa3-c3a2-4a69-a1fd-29dba4b2fde&title=&width=1073.6#averageHue=%23edeaea&crop=0&crop=0&crop=1&crop=1&id=Mv6h0&originHeight=350&originWidth=1342&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)



在 jenkins 工作目录中可以看到打包好的jar包

![img](https://cdn.nlark.com/yuque/0/2022/png/284379/1667789819642-cfbc332e-fb73-4719-9665-e45ec5256ddd.png?x-oss-process=image%2Fresize%2Cw_750%2Climit_0#averageHue=%23022c49&clientId=u64de5965-e773-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=233&id=ua8769ecd&margin=%5Bobject%20Object%5D&name=image.png&originHeight=291&originWidth=785&originalType=binary&ratio=1&rotation=0&showTitle=false&size=27593&status=done&style=none&taskId=ubdfa9063-1b8b-4042-bf4e-58599091e5d&title=&width=628#averageHue=%23022c49&crop=0&crop=0&crop=1&crop=1&id=Nv66c&originHeight=291&originWidth=785&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)



运行jar包

![img](https://cdn.nlark.com/yuque/0/2022/png/284379/1667789862732-760ab5fb-884b-4aeb-a420-6bdcac61f8dd.png?x-oss-process=image%2Fresize%2Cw_750%2Climit_0#averageHue=%23002a46&clientId=u64de5965-e773-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=146&id=u1343137c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=183&originWidth=830&originalType=binary&ratio=1&rotation=0&showTitle=false&size=7652&status=done&style=none&taskId=u8dd3d6cf-f635-421c-b5f8-cecc93ca0f2&title=&width=664#averageHue=%23002a46&crop=0&crop=0&crop=1&crop=1&id=wT2XJ&originHeight=183&originWidth=830&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)



![img](https://cdn.nlark.com/yuque/0/2022/png/284379/1667789906245-7ce2551a-326f-4620-a736-8967881f7dc5.png?x-oss-process=image%2Fresize%2Cw_545%2Climit_0#averageHue=%23dab580&clientId=u64de5965-e773-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=68&id=uf871b58e&margin=%5Bobject%20Object%5D&name=image.png&originHeight=85&originWidth=545&originalType=binary&ratio=1&rotation=0&showTitle=false&size=7255&status=done&style=none&taskId=u61903e7a-f527-442f-adc4-9a5a2d7a6c0&title=&width=436#averageHue=%23dab580&crop=0&crop=0&crop=1&crop=1&id=d0CBz&originHeight=85&originWidth=545&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)





### 4.jenkins开机启动 

每次启动jenkins都要：

![image-20231130162407555](1_jenkins.assets/image-20231130162407555.png)

非常不方便，可以编写脚本，在开机时就启动 jenkins



1创建 jenkins.sh

```
#!/bin/bash
JAVA_HOME=/home/soft/jdk1.8.0_202/bin
 
pid=`ps -ef | grep jenkins.war | grep -v 'grep'| awk '{print $2}'| wc -l`

if [ "$1" = "start" ];then
  if [ $pid -gt 0 ];then
  	echo 'jenkins is running...'
	else
  	### java启动服务 配置java安装根路径,和启动war包存的根路径
  	nohup $JAVA_HOME/java -jar /home/soft/jenkins/jenkins.war > /home/soft/jenkins/jen.log 2>&1 &
  fi
elif [ "$1" = "stop" ];then
  exec ps -ef | grep jenkins | grep -v grep | awk '{print $2}'| xargs kill -9
  echo 'jenkins is stop..'
elif [ "$1" = "restart" ];then
  echo 'jenkins is restart..'
  if [ $pid -gt 0 ];then
  	exec ps -ef | grep jenkins | grep -v grep | awk '{print $2}'| xargs kill -9
  fi
  nohup $JAVA_HOME/bin/java -jar /home/soft/jenkins/jenkins.war > /home/soft/jenkins/jen.log 2>&1 &
else
  echo "Please input like this:"./jenkins.sh start" or "./jenkins stop""
fi
```

![image-20231130162424671](1_jenkins.assets/image-20231130162424671.png)

2设置可执行权限：chmod u+x jenkins.sh

![image-20231130162443059](1_jenkins.assets/image-20231130162443059.png)

3到 /lib/systemd/system 目录下创建 jenkins.service

![image-20231130162456734](1_jenkins.assets/image-20231130162456734.png)

```
[Unit]
Description=Jenkins
After=network.target
 
[Service]
Type=forking
ExecStart=/home/soft/jenkins/jenkins.sh start
ExecReload=
ExecStop=/home/soft/jenkins/jenkins.sh stop
PrivateTmp=true
 
[Install]
WantedBy=multi-user.target
```

刷新服务配置：systemctl daemon-reload 设置开机启动：systemctl enable jenkins.service

![image-20231130162715091](1_jenkins.assets/image-20231130162715091.png)

也可以通过：systemctl start/stop jenkins.service ，启动或关闭服务

![image-20231130162658954](1_jenkins.assets/image-20231130162658954.png)

执行：systemctl status jenkins.service，查看状态

![image-20231130162628241](1_jenkins.assets/image-20231130162628241.png)

### 5.自动发布 

jenkins可以在打包后，自动把jar包发送到服务器中，然后启动

 1. 安装ssh插件 

![image-20231130162808506](1_jenkins.assets/image-20231130162808506.png)





 2. 配置远程服务器 

1选择 系统配置

![image-20231130162824571](1_jenkins.assets/image-20231130162824571.png)





2最下方找到SSH Servers



![img](https://cdn.nlark.com/yuque/0/2022/png/284379/1667790751840-60eb4d77-a210-4856-a057-66132261b136.png?x-oss-process=image%2Fresize%2Cw_697%2Climit_0#averageHue=%23fdf9f9&clientId=u64de5965-e773-4&crop=0&crop=0&crop=1&crop=1&from=paste&height=159&id=u00ddba57&margin=%5Bobject%20Object%5D&name=image.png&originHeight=199&originWidth=697&originalType=binary&ratio=1&rotation=0&showTitle=false&size=11188&status=done&style=none&taskId=ub5725bc2-171a-4799-a142-2a5350bd9f8&title=&width=557.6#averageHue=%23fdf9f9&crop=0&crop=0&crop=1&crop=1&id=sXxU7&originHeight=199&originWidth=697&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)





3添加ssh配置

![image-20231130162850366](1_jenkins.assets/image-20231130162850366.png)





4输入密码：

![img](1_jenkins.assets/image-1701329927016.png)





5测试连接

![image-20231130162916349](1_jenkins.assets/image-20231130162916349.png)





 3. 设置 Post Steps 

![image-20231130162933220](1_jenkins.assets/image-20231130162933220.png)





1构建后通过ssh发送文件

![image-20231130162948757](1_jenkins.assets/image-20231130162948757.png)





2选择服务器，并配置jar文件路径

![image-20231130163001186](1_jenkins.assets/image-20231130163001186.png)



注意：上图中说的test在 /root/.jenkins/workspace路径下





3设置远程上传目录

![image-20231130163017656](1_jenkins.assets/image-20231130163017656.png)





4再次构建

![image-20231130163036992](1_jenkins.assets/image-20231130163036992.png)



5目标服务器查看

![image-20231130163049218](1_jenkins.assets/image-20231130163049218.png)



发现上传的jar包是在 /root/home/target 目录下（自动创建了target目录）





6修改配置

![image-20231130163101173](1_jenkins.assets/image-20231130163101173.png)



再次构建后查看：

![image-20231130163115051](1_jenkins.assets/image-20231130163115051.png)





 4. 运行jar包 

配置上传后，执行命令：

![img](1_jenkins.assets/image-1701329927143.png)



注意：

远程服务器需要提前安装JDK

```
source /etc/profile
nohup java -jar /root/home/*.jar > /root/javasm.log 2>&1 &
```

结果：

![image-20231130163205885](1_jenkins.assets/image-20231130163205885.png)



浏览器也可正常访问

![image-20231130163219378](1_jenkins.assets/image-20231130163219378.png)





 5. Pre Steps 

构建之前，杀掉前一次构建启动的java程序

![image-20231130163250991](1_jenkins.assets/image-20231130163250991.png)



设置执行脚本的命令：./kill.sh daji-git-1.0-SNAPSHOT.jar

![img](1_jenkins.assets/image-1701329927176.png)

注意：

默认会去 /root 目录下，找 kill.sh 文件



在服务器上创建kill.sh

![image-20231130163318603](1_jenkins.assets/image-20231130163318603.png)



```
#!/bin/bash

#删除历史数据
rm -rf home/

#获取执行脚本时传入的参数，比如上图中的gitlab-test-1.0-SNAPSHOT.jar
appname=$1

#获取正在运行的jar包pid
pid=`ps -ef | grep $1 | grep 'java -jar' | awk '{printf $2}'`

#如果pid为空，提示一下，否则，执行kill命令
if [ -z $pid ];
#使用-z 做空值判断
	then
		echo "$appname not started"
	else
		kill -9 $pid
		echo "$appname stoping...."
#检查pid是否被kill掉
check=`ps -ef | grep -w $pid | grep java`
if [ -z $check ];
	then
		echo "$appname pid:$pid is stop"
	else
		echo "$appname stop failed"
fi
fi
```



设置可执行权限：chmod u+x kill.sh



![image-20231130163351969](1_jenkins.assets/image-20231130163351969.png)



再次构建：

![img](1_jenkins.assets/image-1701329945603.png)

![img](1_jenkins.assets/image-1701329945603.png)





## 容器化构建 

外挂目录 

 1. docker启动外部的 jar 包 

使用数据卷映射的方式，docker可以启动外部的 jar 包

```
docker run -d -p 8080:8080 \
-v /root/home/daji-git-1.0-SNAPSHOT.jar:/root/app.jar \
--name=demo1 \
java:8 java -jar /root/app.jar
# 注意：需要先拉取 java:8 的镜像，如果不存在换一个版本
```

![image-20231130163504640](1_jenkins.assets/image-20231130163504640.png)

浏览器访问，结果：

![img](1_jenkins.assets/image-1701333312012.png)

 2. Jenkins配置 

1Pre Steps

构建前，删除上一次的jar包，并暂停容器

![img](1_jenkins.assets/image-1701329945604.png)



2Post Steps

![image-20231130163633246](1_jenkins.assets/image-20231130163633246.png)

 3. 结果 

![img](1_jenkins.assets/image-1701329945605.png)



 2. build docker 镜像 

注意：删除之前的容器和镜像

1在项目中创建dockerfile

![image-20231130163647914](1_jenkins.assets/image-20231130163647914.png)

```
FROM java:8
EXPOSE 8080
WORKDIR /root/home
ADD daji-git-1.0-SNAPSHOT.jar /root/app.jar
ENTRYPOINT ["java","-jar","/root/app.jar"]
```

2Pre Steps
构建前，删除上一次的容器、镜像

![image-20231130163738983](1_jenkins.assets/image-20231130163738983.png)

```
docker rm -f daji
docker rmi daji:1.0
```

3.Post Steps

![img](1_jenkins.assets/image-1701329945617.png)

 4创建一个新的Transfer Set

![image-20231130163830760](1_jenkins.assets/image-20231130163830760.png)



传送dockerfile文件

![img](1_jenkins.assets/image-1701329945619.png)

 

```
cd home
docker build -f dockerfile -t daji:1.0 .
docker run -d -p 8080:8080 --name=daji daji:1.0
```

5构建后结果：

![img](1_jenkins.assets/image-1701329945852.png)





## 问题 

#### 超时 

![image-20231130163914283](1_jenkins.assets/image-20231130163914283.png)

默认超时时间是120秒

如果遇到上面错误，可能：

- 启动时间太长 
  - 如果项目比较大，启动时间会很长，这时可以修改超时时间

![image-20231130163932050](1_jenkins.assets/image-20231130163932050.png)

![image-20231130163952571](1_jenkins.assets/image-20231130163952571.png)

●启动命令不合理 

比如：nohup java -jar /root/home/*.jar &，这个命令会卡住

![image-20231130164017797](1_jenkins.assets/image-20231130164017797.png)



- 修改命令：nohup java -jar /root/home/*.jar > /root/feng.log 2>&1 &

- 启动日志会输入到 /root/feng.log文件中，但这样项目运行时间太长会导致文件很大 

■解决：配置自己的log生成方式，并修改命令：

nohup java -jar /root/home/*.jar > /dev/null 2>&1 & /dev/null：这是一个黑洞文件，往这里写东西，相当于丢弃

#### 文件上传失败 

![image-20231130164037845](1_jenkins.assets/image-20231130164037845.png)

出现上图问题，表示文件路径配置错误，参考下图：

![image-20231130164048545](1_jenkins.assets/image-20231130164048545.png)

#### Cannot run program "java" 

Cannot run program "java" (in directory "/root/.jenkins/workspace/test")

![image-20231130164100876](1_jenkins.assets/image-20231130164100876.png)



jenkins的日志中：hudson.maven.MavenModuleSet cannot be cast to hudson.model.Project

错误报告的很奇怪，使用 java -jar jenkins.war 运行，构建不报错

但是配置 jenkins.service 后，让 jenkins 开机自启动，构建就一直不成功

目前怀疑 jenkins.service 文件有问题，先记录问题，以后再解决

解决：

![image-20231130164113342](1_jenkins.assets/image-20231130164113342.png)



配置JDK就行了

![image-20231130164124468](1_jenkins.assets/image-20231130164124468.png)









## 报告展示

安装html插件

- 安装HTML Publisher plugin插件

![image-20231201171509530](1_jenkins.assets/image-20231201171509530.png)





![image-20231201171811131](1_jenkins.assets/image-20231201171811131.png)



![image-20231201171837403](1_jenkins.assets/image-20231201171837403.png)



因为jenkins默认没有加载css样式，生成的测试报告无样式如下

![image-20231201181344486](1_jenkins.assets/image-20231201181344486.png)

- 处理html报告的样式问题

　　原因分析

　　Jenkins为了避免受到恶意HTML/JS文件的攻击，会默认将安全策略CSP设置为：

　　sandbox; default-src 'none'; img-src 'self'; style-src 'self';

　　在此配置下，只允许加载：

　　1、Jenkins服务器上托管的CSS文件

　　2、Jenkins服务器上托管的图片文件

　　以下形式的内容都会被禁止：

- Java
- plugins (object/embed)
- HTML中的内联样式表（Inline style sheets），以及引用的外站CSS文件
- HTML中的内联图片（Inline image definitions），以及外站引用的图片文件
- frames
- web fonts
- XHR/AJAX

　　解决办法

　　方法一：修改CSP(Content Security Policy)的默认配置，到Jenkins系统管理à脚本命令行，执行以下Groovy命令，然后点击运行。配置完成后，重新构建原有项目，HTML页面即可正常显示。

 ```
System.setProperty("hudson.model.DirectoryBrowserSupport.CSP", "") 
 ```

![image-20231201181426459](1_jenkins.assets/image-20231201181426459.png)

然而当你重新启动jenkins时，你会发现，HTML页面再次面目全非，CSP恢复为默认配置，因此这个办法只是临时方案。

　　方法二：利用jenkins的Groovy 插件永久解决这个问题

　　1、Groovy plugin: 可实现直接执行Groovy代码。

　　解决步骤如下：

　　在“构建”模块，选择“Execute system Groovy script”，执行如下Groovy命令：

 ```
System.setProperty("hudson.model.DirectoryBrowserSupport.CSP", "") 
 ```



 ![image-20231201181448120](1_jenkins.assets/image-20231201181448120.png)

最终的报告样子如下图：

![image-20231201181509497](1_jenkins.assets/image-20231201181509497.png)