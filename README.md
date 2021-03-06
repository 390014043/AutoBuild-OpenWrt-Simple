#                           AutoBuild-OpenWrt-Simple

```
 _    _       __          __     _
| |  | |      \ \        / /    | |  
| |__| | ______\ \  /\  / /_ __ | |_ 
|  __  ||______|\ \/  \/ /| '__|| __|
| |  | |         \  /\  / | |   | |_ 
|_|  |_|          \/  \/  |_|    \__|

项目地址:
https://github.com/390014043/AutoBuild-OpenWrt-Simple
```
| 默认登陆IP | 默认账号 | 默认密码 |
| :-------: | :-------: | :------: |
| 10.0.1.1   | root     | 无 |

| 2.4G_WiFi | 5.0G_WiFi | 默认密码 |
| :-------: | :-------: | :-------: |
|   H-Wrt   | H-Wrt     | 无       |

## 编译教程

### 本地编译教程

1. 安装系统

   推荐 Ubuntu 20.04 LTS x64（全程使用非root用户）

2. 安装依赖

   更换软件源,[清华源官网](https://mirror.tuna.tsinghua.edu.cn/help/ubuntu/)

   ```
     安装依赖，命令行输入# 进行源码备份.
   sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
   # 进行源列表的修改
   sudo vi /etc/apt/sources.list
   
   
   # 将sources.list中的内容提换如下
   # 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
   deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
   # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
   deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
   # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
   deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
   # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
   deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse
   # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse
   
   # 预发布软件源，不建议启用
   # deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
   # deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-proposed main restricted universe multiverse
   
   
   # 查看是否成功
   sudo cat /etc/apt/sources.list
   # 执行更新列表
   sudo apt-get update 
   
   # 更新软件源为最新版，命令行输入
   sudo apt-get update
   
   # 安装依赖，命令行输入
   sudo apt-get -y install build-essential asciidoc binutils bzip2 gawk gettext git libncurses5-dev libz-dev patch python3 python2.7 unzip zlib1g-dev lib32gcc1 libc6-dev-i386 subversion flex uglifyjs git-core gcc-multilib p7zip p7zip-full msmtp libssl-dev texinfo libglib2.0-dev xmlto qemu-utils upx libelf-dev autoconf automake libtool autopoint device-tree-compiler g++-multilib antlr3 gperf wget curl swig rsync
   ```

3. 使用 git克隆源码

   ```
   # 执行以下任何一个即可，推荐使用加速地址，如过感觉速度慢，可以更换一个
   
   # 原git clone 地址
   git clone https://github.com/coolsnowwolf/lede.git
   
   # 加速 git clone 地址
   git clone https://github.do/https://github.com/coolsnowwolf/lede.git
   git clone https://api.mtr.pub/coolsnowwolf/lede.git
   git clone https://hub.fastgit.xyz/coolsnowwolf/lede.git
   git clone https://ghproxy.com/https://github.com/coolsnowwolf/lede.git
   git clone https://gh.gcdn.mirr.one/coolsnowwolf/lede.git
   git clone https://hub.0z.gs/coolsnowwolf/lede.git
   git clone https://hub.shutcm.cf/coolsnowwolf/lede.git
   ```


4. 更新 openwrt 系统的软件源

   ```
   cd ./lede
   ./scripts/feeds update -a
   ./scripts/feeds install -a
   ./scripts/feeds install -a
   ```
   
5. 配置编译选项

   ```
   # 打开配置菜单、在这个界面你可以，按照自己的要求来配置属于你自己的openwrt
   make menuconfig
   ```

6. 下载dl库（国内请尽量全局科学上网）

   ```
   make -j8 download V=s
   ```

7. 进行编译

   ```
   make -j1 V=s
   # -j1 后面是线程数。第一次编译推荐用单线程
   ```

本套代码保证肯定可以编译成功。里面包括了 R21 所有源代码，包括 IPK 的

大部分人失败的原因是因为网络原因

### 云编译教程

云编译：编译工作由[GitHub](https://github.com/) 的 Gitflow Workflow（github工作流）完成

1. 安装系统，推荐 Ubuntu 20.04 LTS x64（全程使用非root用户）

2. 安装依赖

3. 使用 git克隆源码、更新 openwrt 系统的软件源（同上）

   此过程由于需要从GitHub下载、更新源码，考虑到大部分小伙伴网络情况不一

   本人提供了一个更新好的源码

  ```shell
源码安装更新时间：
11:50 2022年4月2日（星期六） (GMT+8)
链接：
http://
# 下载后，上传并解压源码
tar zxvf lede.tar.gz
# 解压后更新源码（注意路径）
cd ~/lede
git pull
cd ~/lede/package/OpenClash
git pull
cd ~/lede/package/luci-app-adguardhome
git pull
cd ~/lede/package/lean/luci-theme-argon-18.06
git pull
  ```

4. 制作配置文件

   `make menuconfig`

   配置完成后，会在lede目录下生成.config配置文件

   由.config文件生成差异文件（只记录与默认文件差异项）

   `make diffconfig`

   ```
   生成的差异文件会在相应的架构下
   bin/targets/**/**/config.buildinfo
   例如：
   bin/targets/x86/64/config.buildinfo
   ```

5. fork(复制)仓库

     注册登录GitHub、进入下面链接、在右上角点击fork，即可将本仓库复制到你的GitHub里面

     https://github.com/390014043/AutoBuild-OpenWrt-Simple

6. 上传配置文件

   将自己 AutoBuild-OpenWrt-Simple 仓库内的 openwrt.config 文件内容替换为差异文件的内容

7. 触发编译

   随便更改 logs/openwrt.md 的 内容 即可

祝大家编译成功

### 进阶

相信大家已经编译好了自己固件、对编译过程有了一个了解

下面介绍进阶玩法，再次之前呢，给大家卖个关子、下期见

banner、customize.sh、docs/wifi.txt、Build_OP_openwrt.yml等文件等着大家去探索哦

## 方便自己的一键系列

### 拉取源码

```
git clone https://github.com/coolsnowwolf/lede.git ~/lede \
&& git clone https://github.com/vernesong/OpenClash.git ~/lede/package/OpenClash \
&& git clone https://github.com/rufengsuixing/luci-app-adguardhome.git ~/lede/package/luci-app-adguardhome \
&& git clone https://github.com/jerrykuku/luci-theme-argon.git ~/lede/package/lean/luci-theme-argon-18.06
```

### 拉取最新源码

```
cd ~/lede && git pull \
cd ~/lede/package/OpenClash && git pull \
cd ~/lede/package/luci-app-adguardhome && git pull \
cd ~/lede/package/lean/luci-theme-argon-18.06 && git pull
```

### 更新 openwrt 系统的软件源

```
cd ./lede
./scripts/feeds update -a
./scripts/feeds install -a
./scripts/feeds install -a
```

### DD更新系统

```
dd if=/tmp/upload/openwrt.img of=/dev/sda
```

## 使用docker编译与云编译

### docker 安装

使用官方安装脚本自动安装

```
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
```

### docker 镜像加速

使用docker的时候，总是需要去search镜像，使用国外的源下载太慢，还有诸多的限制，可以使用阿里云进行加速，操作如下：

1. 打开阿里云控制台，没有的可以用淘宝账号或者支付宝账号直接登录

https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors

打开容器镜像服务，镜像加速器，复制加速地址后，按照第二个红框所示，完成配置。

![img](https://gitee.com/Huzy_led/shoulders-of-giants/raw/master/imgs/1592465-20190721112607382-476284117.png)

2. 重启docker

systemctl daemon-reload

systemctl restart docker

OK!加速完成，嗖嗖嗖！！！

### docker 镜像拉取

```
# 镜像比较大请大家耐心等待
docker pull hzyyd/ubuntu-20.04-lede

docker run -itd --name ubuntu-20.04-lede hzyyd/ubuntu-20.04-lede
```

### 进入docker容器

```
docker exec -it ubuntu-20.04-lede bash

# 推出docker容器
按住ctrl，然后按d，即可推出容器
```

### docker容器目录

```
# 首先解压lede源码
cd ~ && tar zxvf lede.tgz

# 添加 git 安全目录
git config --global --add safe.directory /root/lede
git config --global --add safe.directory /root/lede/package/OpenClash
git config --global --add safe.directory /root/lede/package/luci-app-adguardhome
git config --global --add safe.directory /root/lede/package/lean/luci-theme-argon-18.06

# 更新源码
cd ~/lede && git pull
cd ~/lede/package/OpenClash && git pull
cd ~/lede/package/luci-app-adguardhome && git pull
cd ~/lede/package/lean/luci-theme-argon-18.06 && git pull

# 然后就可以向上面一样愉快的编译与云编译了

# lede所在目录
~/lede && git pull
# OpenClash所在目录
~/lede/package/OpenClash
# luci-app-adguardhome所在目录
~/lede/package/luci-app-adguardhome
# luci-theme-argon-18.06所在目录
~/lede/package/lean/luci-theme-argon-18.06
```

## 目录结构

```
AutoBuild-OpenWrt-Simple        //
├─ banner                       //
├─ customize.sh                 //
├─ docs                         //
│  └─ wifi.txt                  //
├─ logs                         //
│  ├─ openwrt.md                //
│  ├─ redmi_ac2100.md           //
│  ├─ rpi4_64.md                //
│  ├─ x86_64.md                 //
│  ├─ xiaomi_ac2100.md          //
│  ├─ xiaomi_r3g.md             //
│  └─ xiaomi_r4a.md             //
├─ openwrt.config               //
├─ README.md                    //
├─ redmi_ac2100.config          //
├─ redmi_ac2100_def.config      //
├─ rpi4_64.config               //
├─ rpi4_64_def.config           //
├─ x86_64.config                //
├─ x86_64_def.config            //
├─ xiaomi_ac2100.config         //
├─ xiaomi_ac2100_def.config     //
├─ xiaomi_r3g.config            //
├─ xiaomi_r3g_def.config        //
├─ xiaomi_r4a.config            //
└─ xiaomi_r4a_def.config        //
```

## 感谢

- [大雕 源码仓库](https://github.com/coolsnowwolf/lede.git) ©coolsnowwolf
- [esir 自动编译](https://github.com/esirplayground/AutoBuild-OpenWrt.git) ©esirplayground
- [OpenClash 源码](https://github.com/vernesong/OpenClash.git)  © vernesong
- [adguardhome 源码](https://github.com/rufengsuixing/luci-app-adguardhome.git) ©rufengsuixing
- [luci-theme-argon 源码](https://github.com/jerrykuku/luci-theme-argon.git) ©jerrykuku
- [缓存加速编译](https://github.com/klever1988/cachewrtbuild) ©klever1988