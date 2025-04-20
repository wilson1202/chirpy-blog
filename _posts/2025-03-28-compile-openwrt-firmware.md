---
title: OpenWrt 官方源码编译固件
author: yongren
date: 2025-03-28 13:58:07 +0800
categories: [系统, OpenWrt]
tags: [OpenWrt, 编译, 路由器]
pin: false
---

## 官方地址

> 源码：[https://github.com/openwrt/openwrt](https://github.com/openwrt/openwrt)
>
> 固件：[https://downloads.openwrt.org](https://downloads.openwrt.org/)

## 准备工作

### 依赖

以 `root` 用户或者有 `root` 权限的用户执行以下命令

直接抄大佬的环境  [Requirements](https://github.com/immortalwrt/immortalwrt?tab=readme-ov-file#requirements) 

```bash
sudo apt update -y
sudo apt full-upgrade -y
sudo apt install -y ack antlr3 asciidoc autoconf automake autopoint binutils bison build-essential \
  bzip2 ccache clang cmake cpio curl device-tree-compiler ecj fastjar flex gawk gettext gcc-multilib \
  g++-multilib git gnutls-dev gperf haveged help2man intltool lib32gcc-s1 libc6-dev-i386 libelf-dev \
  libglib2.0-dev libgmp3-dev libltdl-dev libmpc-dev libmpfr-dev libncurses-dev libpython3-dev \
  libreadline-dev libssl-dev libtool libyaml-dev libz-dev lld llvm lrzsz mkisofs msmtp nano \
  ninja-build p7zip p7zip-full patch pkgconf python3 python3-pip python3-ply python3-docutils \
  python3-pyelftools qemu-utils re2c rsync scons squashfs-tools subversion swig texinfo uglifyjs \
  upx-ucl unzip vim wget xmlto xxd zlib1g-dev zstd
```

或者

```bash
sudo bash -c 'bash <(curl -s https://build-scripts.immortalwrt.org/init_build_environment.sh)'
```

### 源码

以下命令以普通用户执行，不要用 `root`

- 下载源码

    ```bash
    git clone -b v24.10.0 --single-branch --depth=1 https://github.com/openwrt/openwrt.git
    ```
    > `-b` 指定分支/标签、 `--single-branch` 仅下载单个分支/标签、 `--depth=1` 只下载最新的一次提交 
    {: .prompt-info }

- 进入目录

    ```
    cd openwrt
    ```

### 自定义时区

```bash
# 修改时区
sed -i '/set system.@system\[\-1\].timezone=.*/c\        set system.@system[-1].timezone='\''CST-8'\''' package/base-files/files/bin/config_generate
# 设置时区名称
sed -i '/set system.@system\[\-1\].timezone=.*/a\        set system.@system[-1].zonename='\''Asia/Shanghai'\''' package/base-files/files/bin/config_generate
```

### 自定义IP

默认管理`IP`为 `192.168.1.1` ，可以根据自己的需求进行修改

```bash
sed -i 's/192.168.1.1/10.0.0.2/g' package/base-files/files/bin/config_generate
```

### 自定义软件包

- 将软件包仓库地址写入 `feeds.conf.default` 文件中
  - 添加 [`helloworld`](https://github.com/fw876/helloworld)

    ```bash
    sed -i "/helloworld/d" "feeds.conf.default"
    echo "src-git helloworld https://github.com/fw876/helloworld.git" >> "feeds.conf.default"
    ```

- 拉取软件包

	```bash
    ./scripts/feeds update -a
    ./scripts/feeds install -a
  ```

- 使用 `git clone` 下载或升级软件包

  - 升级 [`golang`](https://github.com/kenzok8/golang) ，[`helloworld`](https://github.com/fw876/helloworld) 项目必备

    ```bash
    rm -rf feeds/packages/lang/golang
    git clone https://github.com/kenzok8/golang feeds/packages/lang/golang
    ```

	
  - 添加 [`luci-theme-argon`](https://github.com/jerrykuku/luci-theme-argon)

    ```bash
    git clone https://github.com/jerrykuku/luci-theme-argon.git feeds/luci/themes/luci-theme-argon
    ```

  - 升级 [`openwrt-smartdns`](https://github.com/pymumu/openwrt-smartdns) 和 [`luci-app-smartdns`](https://github.com/pymumu/luci-app-smartdns)

    ```bash
    rm -rf feeds/packages/net/smartdns
    git clone https://github.com/pymumu/openwrt-smartdns.git feeds/packages/net/smartdns
    rm -rf feeds/luci/applications/luci-app-smartdns
    git clone https://github.com/pymumu/luci-app-smartdns.git feeds/luci/applications/luci-app-smartdns
    ```

### 菜单配置

执行以下命令打开菜单配置

```bash
make menuconfig
```

按照如下 `配置` 进行选择，其他未说明的不要动

```tex
.
├── Target System -> `x86`
├── Subtarget -> `x86_64`
├── Target Images
│   ├── < > `tar.gz`
│   ├── < > `ext4`
│   ├── < > `Build GRUB images`
│   ├── (1) `Seconds to wait before booting the default entry`
│   └── (256) `Root filesystem partition size`
├── Base system
│   ├── < > `dnsmasq`                # 二选一
│   └── <*> `dnsmasq-full`           # 二选一
├── Libraries
│   ├── < > `libustream-mbedtls`
│   ├── <*> `libustream-openssl`     # 与 [helloworld] 保持一致
│   └── < > `libustream-wolfssl`
├── LuCI
│   ├── Collections
│   │   └── <*> `luci`
│   ├── Modules
│   │   ├── Translations
│   │   │   └── <*> `Chinese Simplified`
│   │   └── <*> `luci-compat`        # [helloworld] 勾选
│   └── Applications
│       └── [按需选择软件包]
└── Network
    ├── SSH
    │   └── <*> `openssh-sftp-server`
    └── <*> `snmpd`
```

### 预下载

这里会先把编译时要下载的文件下载下来（只是一部分，后面编译阶段还会下载，仍需保持网络畅通）

```bash
make download -j8 V=s
```

## 编译

### 执行编译

根据CPU核心数开启指定线程数

```bash
make V=s -j$(nproc)
```

> 如果编译过程中报错了，可以使用单线程模式 `make V=s -j1` ，确认出错的地方，然后排查（注：有时候单线程就不报错了），使用 `-j` 参数可以指定编译时使用的线程数 
{: .prompt-info }

### 二次编译

```bash
# 进入目录
cd openwrt
# 拉取更新
git pull
# 更新系统组件
./scripts/feeds update -a && ./scripts/feeds install -a
# 生成配置文件
make defconfig
# 下载源码文件
make download -j8
# 使用 WSL/WSL2 进行编译
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
# 执行编译
make V=s -j$(nproc)
```

### 如果需要重新配置

```bash
# 清除缓存和配置文件
rm -rf ./tmp && rm -rf .config
# 打开配置菜单
make menuconfig
# 下载源码文件
make download -j8 
# 使用 WSL/WSL2 进行编译
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
# 执行编译
make V=s -j$(nproc)
```

### 生成 `seed.config` 文件

```bash
make defconfig
./scripts/diffconfig.sh > seed.config
```




## 参考

[使用 OpenWrt 23.05.5 官网源码编译固件](https://maxqiu.com/article/detail/153)
