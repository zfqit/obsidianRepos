## 前置条件

* 安装JDK8
* 安装 Maven
* 安装Git

## Jenkins 安装
>  [Jenkins 官网安装稳定版](https://www.jenkins.io/download/)
```shell 
# 安装稳定版本 Debian 系统
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key

yum install fontconfig java-11-openjdk
yum install jenkins
```

```shell
# 安装稳定版本 CentOS 系统
wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key

yum install fontconfig java-11-openjdk
yum install jenkins
```

## 配置Jenkins

* 初始化jenkins
```shell
systemctl start jenkins
# 然后访问 http::/ip:8080
# 登录需要输入密码,跟着图形化界面操作
```
* 安装插件
	* `Maven Integration`
	* `Pipeline Maven Integration`
	* `Gitlab`
	* `GitLab Authentication`
	* `SSH`
	* `Publish Over SSH`
* 添加凭据 Manage Credentials（凭据管理）
![](http://img.zfqit.top/img/202206031439783.png)

* 配置SSH
![](http://img.zfqit.top/img/202206031436050.png)

![](http://img.zfqit.top/img/202206031437730.png)
![](http://img.zfqit.top/img/202206031437475.png)
![](http://img.zfqit.top/img/202206031438842.png)
* 配置maven
![](http://img.zfqit.top/img/202206031442095.png)

![](http://img.zfqit.top/img/202206031442133.png)

* 配置Java
![](http://img.zfqit.top/img/202206031443762.png)

* 配置Git
![](http://img.zfqit.top/img/202206031443151.png)

## 构建Jenkins
* 配置Git
![](http://img.zfqit.top/img/202206031448432.png)
* 构建触发器
![](http://img.zfqit.top/img/202206031449111.png)
![](http://img.zfqit.top/img/202206031449682.png)

![](http://img.zfqit.top/img/202206031450213.png)
![](http://img.zfqit.top/img/202206031451151.png)

* 构建环境
![](http://img.zfqit.top/img/202206031452020.png)

```shell
SERVER_NAME_1=jct-debug
echo "=========================>>>>>>>工作空间WORKSPACE的地址：$WORKSPACE "
cd $WORKSPACE
echo "=========================>>>>>>>进入工作空间WORKSPACE，清除工作空间中原项目的工作空间$SERVER_NAME_1 "
rm -rf $SERVER_NAME_1
echo "=========================>>>>>>>清除工作空间中原项目的工作空间$SERVER_NAME_1 ......成功success"
```
* Build
设置 maven 打包
```shell
clean package -Dmaven.test.skip=true
```
![](http://img.zfqit.top/img/202206031453340.png)

![](http://img.zfqit.top/img/202206031455312.png)

![](http://img.zfqit.top/img/202206031455475.png)

```shell

#!/bin/bash 

echo '开始maven 构建服务'

#export BUILD_ID=dontKillMe这一句很重要，这样指定了，项目启动之后才不会被Jenkins杀掉。
export BUILD_ID=dontKillMe

mvn clean install

#指定最后编译好的jar存放的位置
www_path=/jct/jct/

#Jenkins中编译好的jar位置
jar_path=/var/lib/jenkins/workspace/jct-debug/jct-admin/target

cd  ${jar_path}

#获取Jenkins中编译好的jar名称，其中XXX为你的pom文件中的artifactId的值，这一步主要是为了根据项目版本号动态获取项目文件名
#jar_name=`ls |grep XXX-|grep -v original`
jar_name=`ls |grep jct-admin.jar -|grep -v original`
#jar_name=mybatis-generator-1.0-SNAPSHOT.jar

#需要注意的是，初次构建时并没有对应的pid，所以需要判断一下是否存在该文件
#获取运行编译好的进程ID，便于我们在重新部署项目的时候先杀掉以前的进程
if [ -f "/jct/jct/jct-admin.pid" ];then
  pid=$(cat /jct/jct/jct-admin.pid)
  #杀掉以前可能启动的项目进程
  kill -9 ${pid}
fi

#进入最后指定存放jar的位置
cd  ${www_path}

#先删除原来的jar文件
rm -rf ./${jar_name}

#进入指定的编译好的jar的位置
cd  ${jar_path}

#将编译好的jar复制到最后指定的位置
cp  -r ${jar_path}/${jar_name} ${www_path}

#进入最后指定存放jar的位置
cd  ${www_path}

#启动jar，指定SpringBoot的profiles为beta,后台启动
#java -jar -Dspring.profiles.active=beta ${jar_name} &
# java -jar ${jar_name} &
nohup java -jar -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005  ${www_path}jct-admin.jar > spring.log 2>&1 &


#将进程ID存入到ufind-web.pid文件中
echo $! > /jct/jct/jct-admin.pid
```

## 参考文章
> [小白学习gitlab+jenkins进行自动化构建(docker)之springboot项目](https://juejin.cn/post/7073478980099112996#heading-11)
> [从零学习Jenkins部署SpringBoot项目](https://juejin.cn/post/7012971055182512135#heading-13)

