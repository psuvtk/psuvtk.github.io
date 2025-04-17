# {OpenGrok引擎浏览开源代码


&lt;!--more--&gt;

[TOC]

阅读大型源码项目除传统的Souce Insight、Scitools Understand等付费PC端工具，基于Web的 [{OpenGrok](https://oracle.github.io/opengrok/) 和 [Elixir Cross Reference](https://elixir.bootlin.com/) 也是相当惊艳，方便躺在床上用ipad阅读源代码。作为开源工具，不仅免费而且支持用户自己搭建。如下为{OpenGrok的界面:

![w](https://upload-images.jianshu.io/upload_images/4572693-4d729029d88ca196.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

另外，二者都提供许多常见的大型项目([https://github.com/oracle/opengrok/wiki/Installations](https://github.com/oracle/opengrok/wiki/Installations)
以及[https://elixir.bootlin.com/](https://elixir.bootlin.com/))，足够使用并且免除动手搭建的麻烦。但毕竟是国外网站，偶尔访问速度上不去，故自己动手搭建一套还是非常有用的。OpenGrok的源码方式安装还是蛮麻烦的，所以使用官方的Docker镜像并且自己写了个[小脚本](https://github.com/psuvtk/install_opengrok_docker)方便进行管理。

## 1. Debian/Ubuntu系统搭建{OpenGrok
博主使用的是Debian系统，所以包管理器为`apt`，如果你使用其它系统或者想要改变**端口**或者**源码目录**，简单修改脚本即可。
(1). 安装 Docker / Opengrok
``` bash
./opengrok.sh install
```
该命令做了如下工作: 删除已存在的Docker、启用https、加入docker的官方gpg密钥、校验密钥、添加稳定源、安装Docker社区版、安装opengrok/docker image、创建opengrok目录。
``` bash
sudo apt remove docker docker-engine docker.io containerd runc
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
sudo add-apt-repository \
	&#34;deb [arch=amd64] https://download.docker.com/linux/debian \
	$(lsb_release -cs) \
	stable&#34;
sudo apt update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io
sudo docker pull opengrok/docker
sudo mkdir -p $GROKPATH/etc
sudo mkdir -p $GROKPATH/data
sudo mkdir -p $GROKPATH/src
```

(2). 运行opengrok
``` bash
./opengrok.sh run
```
该命令执行以下工作：停止正在的运行的opengrok/docker进程、删除之前生成的配置和数据(**可选**)、后台运行opengrok/docker映像。
``` bash
sudo docker stop $CONTAINER_NAME
sudo docker rm $CONTAINER_NAME

sudo rm -rf $GROKPATH/etc/*
sudo rm -rf $GROKPATH/data/*

%模板
sudo docker run -d  \
	 --name $CONTAINER_NAME \
	 -p $PORT:8080/tcp \
	 -e REINDEX=$REINDEX \
	 -v $GROKPATH/src/:/opengrok/src/ \
	 -v $GROKPATH/etc/:/opengrok/etc/ \
	 -v $GROKPATH/data/:/opengrok/data/ \
	 opengrok/docker:latest

sudo docker run -d  \
     --name &#34;opengrok&#34; \
     -p 8888:8080/tcp \
     -e REINDEX=&#34;0&#34; \
     -v ~/opengrok/src/:/opengrok/src/ \
     -v ~/opengrok/etc/:/opengrok/etc/ \
     -v ~/opengrok/data/:/opengrok/data/ \
     opengrok/docker:latest
```

(3). 如果添加了其他项目，运行如下命令重新生成索引
``` bash
./opengrok.sh reindex
```
或
``` bash
docker exec &lt;container&gt; /scripts/index.sh
```

## 完整脚本
```shell
#!/bin/bash

GROKPATH=~/opengrok
CONTAINER_NAME=opengrok
PORT=8888
REINDEX=&#34;0&#34;

function pr_info()
{
	echo -e &#34;\033[32m&#34; $1 &#34;\033[0m&#34;
}

# install docker community edition and pull opengrok official image
function install()
{
	sudo apt install -y curl
	sudo apt remove docker docker-engine docker.io containerd runc
	curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
	sudo apt-key fingerprint 0EBFCD88
	sudo add-apt-repository \
	   &#34;deb [arch=amd64] https://download.docker.com/linux/debian \
	   $(lsb_release -cs) \
	   stable&#34;
	sudo apt update
	sudo apt-get install -y docker-ce docker-ce-cli containerd.io
	sudo docker pull opengrok/docker
	sudo mkdir -p $GROKPATH/etc
	sudo mkdir -p $GROKPATH/data
	sudo mkdir -p $GROKPATH/src
	pr_info &#34;\nDone!&#34;
}

function run()
{
	echo &#34;check exist docker opengrok...&#34;
	sudo docker stop $CONTAINER_NAME
	sudo docker rm $CONTAINER_NAME

	echo &#34;delete existing data and etc...&#34;
	sudo rm -rf $GROKPATH/etc/*
	sudo rm -rf $GROKPATH/data/*
	
	echo &#34;run docker image opengrok/docker:latest&#34;
	sudo docker run -d  \
	    --name $CONTAINER_NAME \
	    -p $PORT:8080/tcp \
	    -e REINDEX=&#34;60&#34; \
	    -v $GROKPATH/src/:/opengrok/src/ \
	    -v $GROKPATH/etc/:/opengrok/etc/ \
	    -v $GROKPATH/data/:/opengrok/data/ \
	    opengrok/docker:1.5.12
	pr_info &#34;\n opengrok/docker running.&#34;	
}

# Reindex When you add some new Project 
function reindex()
{
	sudo docker exec opengrok /scripts/index.sh &amp;
	pr_info &#34;\nDone!&#34;
}

function usage()
{
	echo -e &#34;\033[31m&#34; &#34;\nusage:&#34; &#34;\t./opengropk.sh [install|run|reindex|usage]&#34;
	echo -e &#34;\tinstall\t install docker community edition and opengrok official image&#34;
	echo -e &#34;\trun\trun opengrok at specific port&#34;
	echo -e &#34;\treindex\treindex when you add some new project in \${GROKPATH}/src directory&#34;
	echo -e &#34;\033[0m&#34;
}

if [ $# -eq 0 ]; then 
	usage
fi

case $1 in
	&#34;install&#34;)
		install	
		;;
	&#34;run&#34;)
		run
		;;
	&#34;reindex&#34;)
		reindex
		;;
	*)
		usage
		;;
esac
```


## 2. 剧透一下，Windows 也可以搭建哦~ , 适合一些只能在本机看的私有项目
(1) 下载并安装[Docker For Windows](https://hub.docker.com/?overlay=onboarding);
(2) 启动 Docker Desktop 守护进程；
(3) 在合适的位置创建opengrok文件夹，比如：`d:/docker/opengrok/`，并创建三个子文件夹`etc`、`data`、`src`；
(4) 将源代码解压到`opengrok/src`文件夹；
(5) 运行如下命令运行opengrok映像
``` bash
docker run -d --name &#34;opengrok&#34; -p 8888:8080/tcp -e REINDEX=&#34;0&#34; -v d:/docker/opengrok/src/:/opengrok/src/ -v d:/docker/opengrok/etc/:/opengrok/etc/ -v d:/docker/opengrok/data/:/opengrok/data/  opengrok/docker:latest 
```
(6) 若后续添加源代码则运行如下命令进行重新索引
``` bash
docker exec opengrok /scripts/index.sh
```
**注** ：Hyper-V会与VirtualBox冲突
解决方法:
(a). 关闭Hyper-V，管理员终端运行如下命令；
``` bash
bcdedit /set hypervisorlaunchtype off
```
(b). 若还是要使用Docker可以尝试使用旧版的[Docker ToolBox](https://docs.docker.com/toolbox/toolbox_install_windows/)；
(c). 尝试在WSL或者虚拟机里面的Linux安装Docker/OpenGrok。



# 2022 更新

```
CONTAINER ID   IMAGE                    COMMAND               CREATED        STATUS        PORTS                                       NAMES
f7632745dfe1   opengrok/docker:1.5.12   &#34;/scripts/start.sh&#34;   2 months ago   Up 2 months   0.0.0.0:8888-&gt;8080/tcp, :::8888-&gt;8080/tcp   opengrok
```

最新版本的opengrok镜像命令字好像与之前并不一样，这里找到与之前命令一样的 1.5.12 版本, 拉取指定版本的opengrok镜像进行使用。


## 参考
1. [https://github.com/OpenGrok/docker](https://github.com/OpenGrok/docker)

---

> Author: Kristoffer  
> URL: http://localhost:1313/posts/76052be/  

