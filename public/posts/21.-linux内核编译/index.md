# 内核编译


1. 下载Ubuntu镜像
2. 安装Virtual Box
3. 下载内核源码, `linux-source` 这个包可能和当前的内核版本不匹配？, 所以使用`linux-source-5.xx.x`包
```sh
sudo apt-get install linux-headers-$(uname -r)
sudo apt-get install linux-source-5.xx.x
cd /usr/src/linux-source-5.19.0
sudo tar jxf /usr/src/linux-source-5.19.0.tar.bz2
```

## 依赖安装
```sh
sudo apt install build-essential libncurses5-dev libssl-dev flex bison
```

## 编译内核
```sh
#清除之前编译环境
make mrproper
# 编译
sudo make defconfig
sudo make -j4
```


##

安装基本软件
```
sudo apt install zsh git curl vim
```

---

> Author: Kristoffer  
> URL: https://psuvtk.github.io/posts/21.-linux%E5%86%85%E6%A0%B8%E7%BC%96%E8%AF%91/  

