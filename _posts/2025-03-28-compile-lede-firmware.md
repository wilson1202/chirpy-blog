---
title: LEDE 源码编译固件
author: yongren
date: 2025-03-28 15:58:07 +0800
categories: [系统, OpenWrt]
tags: [openwrt, lede, 编译, 路由器]
pin: false
---

## 项目地址

> [Lean的Openwrt源码仓库](https://github.com/coolsnowwolf/lede)

> **编译注意事项**
>
> 1. 不要用 `root` 用户进行编译！！！
> 2. 国内用户编译前最好准备好梯子
> 3. 默认登陆IP `192.168.1.1` 密码 `password`
{: .prompt-info }

## 编译命令

1. 首先装好 Linux 系统，推荐 Debian 或 Ubuntu LTS

2. 安装编译依赖

    ```bash
    sudo apt update -y
    sudo apt full-upgrade -y
    sudo apt install -y ack antlr3 asciidoc autoconf automake autopoint binutils bison build-essential \
    bzip2 ccache clang cmake cpio curl device-tree-compiler flex gawk gcc-multilib g++-multilib gettext \
    genisoimage git gperf haveged help2man intltool libc6-dev-i386 libelf-dev libfuse-dev libglib2.0-dev \
    libgmp3-dev libltdl-dev libmpc-dev libmpfr-dev libncurses5-dev libncursesw5-dev libpython3-dev \
    libreadline-dev libssl-dev libtool llvm lrzsz msmtp ninja-build p7zip p7zip-full patch pkgconf \
    python3 python3-pyelftools python3-setuptools qemu-utils rsync scons squashfs-tools subversion \
    swig texinfo uglifyjs upx-ucl unzip vim wget xmlto xxd zlib1g-dev
    ```

    > 如果使用 `root` 执行了以上命令，那接下来必须使用 `非root` 权限用户进行后续操作
    {: .prompt-warning  }
    
3. 下载好源代码

    ```bash
    git clone https://github.com/coolsnowwolf/lede
    ```

4. 进入 lede 目录（源码存在此目录，所以此目录即为 buildroot 目录）

   ```bash
   cd lede
   ```

5. 更新系统组件

    ```bash
    ./scripts/feeds update -a
    ./scripts/feeds install -a
    make menuconfig
    ```

    > 如果已经成功编译过，再次编译时还要 `git pull` 同步源码
    {: .prompt-tip  }
    
6. 运行 `make menuconfig` 入选单界面，选择 CPU 架构，型号，固件类型，所需插件及工具等，记得先 `Save` 再退出

7. 下载源码文件到 `buildroot` 录下的 `dl` 目录

    ```bash
    make download -j8
    ```

8. `WSL` 编译前可尝试运行以下代码

    ```bash
    PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
    ```

9. 正式开始编译，建议先运行 `screen` 命令守护进程，尤其是在 `VPS` 上编译时（ `-j` 后面是线程数，第一次编译推荐用单线程）

    ```bash
    make V=s -j1
    ```

## 二次编译

```bash
cd lede
git pull
./scripts/feeds update -a && ./scripts/feeds install -a
make defconfig
make download -j8
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
make V=s -j$(nproc)
```


## 重新配置

```bash
rm -rf ./tmp && rm -rf .config
make menuconfig
make download 
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
make V=s -j$(nproc)
```


## 生成 seed.config 文件

```bash
make defconfig
./scripts/diffconfig.sh > seed.config
```

