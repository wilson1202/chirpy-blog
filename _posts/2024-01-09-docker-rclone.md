---
title: Docker部署Rclone
author: yongren
date: 2024-01-09 21:55:19 +0800
categories: [七零八落的折腾, Docker]
tags: [docker, rclone, rclonebrowser]
pin: false
image:
  path: images/2024-01-09-docker-rclone/docker-rclone.png
---

## 部署简介

1. 部署说明
2. 部署项目 - rclone（推荐）
3. 部署项目 - rclonebrowser
4. 群晖设置

## 部署说明

1. rclone 项目部署前，需要完成网盘 WebDAV 项目的部署，这是前提，推荐两个方案

   > 方案1：aliyundrive-webdav，Docker Hub 项目地址 [messense/aliyundrive-webdav](https://hub.docker.com/r/messense/aliyundrive-webdav)
   >
   > 方案2：xhofe/alist，Docker Hub 项目地址 [xhofe/alist](https://hub.docker.com/r/xhofe/alist)

2. rclone 和 rclonebrowser 二选一即可，推荐 rclone 项目，rclonebrowser 项目已一年未更新

3. 部署环境：群晖 DS918，DSM 7.2

## 部署项目 - rclone

1. 安装 rclone 二进制文件

    ```
    curl https://rclone.org/install.sh | sudo bash
    ```

    > 官方文档：[Install (rclone.org)](https://rclone.org/install/)   

2. 查看 rclone 版本，`rclone version`

    ![image-20240109224303841](images/2024-01-09-docker-rclone/image-20240109224303841.png)

3. 配置 config 文件，`rclone config`

    ![Snipaste_2024-01-09_21-00-09](images/2024-01-09-docker-rclone/Snipaste_2024-01-09_21-00-09.png)

    ![Snipaste_2024-01-09_21-01-24](images/2024-01-09-docker-rclone/Snipaste_2024-01-09_21-01-24.png)

    ![Snipaste_2024-01-09_21-03-09](images/2024-01-09-docker-rclone/Snipaste_2024-01-09_21-03-09.png)

    ![Snipaste_2024-01-09_21-06-13](images/2024-01-09-docker-rclone/Snipaste_2024-01-09_21-06-13.png)

    > 该版本总共10处需要手动输入，序号7漏掉，步骤未漏，代码行数可以衔接，密码确认之后的步骤，全部回车即可

    ![Snipaste_2024-01-09_21-08-36](images/2024-01-09-docker-rclone/Snipaste_2024-01-09_21-08-36.png)

    > 完成配置，界面会展示部分配置信息

4. 查看 config 文件路径，`rclone config file`

    ![image-20240109230300683](images/2024-01-09-docker-rclone/image-20240109230300683.png)

    > 文件位置：`/root/.config/rclone`，保存好后面要用

5. 查看 config 文件内容，`cat /root/.config/rclone/rclone.conf`

    ![image-20240109230621143](images/2024-01-09-docker-rclone/image-20240109230621143.png)

    > 修改文件使用 `nano` 或 `vim` 命令
    >
    > `[anliyun]` 是 rclone 配置文件中的 `name` ，也是下步项目部署时的 `<云端名称>` ，实际两处保持一致即可

6. 部署 rclone 项目

    ```yaml
    version: '3.3'
    services:
      rclone:
        container_name: rclone
        image: rclone/rclone
        command: mount aliyun:IIVA /data --allow-other --allow-non-empty --vfs-cache-mode writes
        restart: always
        volumes:
          - /root/.config/rclone/:/config/rclone
          - /volume2/video/aliyun:/data:shared
          - /etc/passwd:/etc/passwd:ro
          - /etc/group:/etc/group:ro
        devices:
          - /dev/fuse
        cap_add:
          - SYS_ADMIN
        security_opt:
          - apparmor:unconfined
        user: "${UID}:${GID}"
    ```

    > Docker Hub 项目地址：[rclone/rclone](https://hub.docker.com/r/rclone/rclone)
    >
    > `mount aliyun:IIVA /data`：mount `<云端名称>`:`<实际云端目录>` /`<容器目录>`
    >
    > `- /root/.config/rclone`：rclone config 配置文件目录
    >
    > `- /volume2/video/aliyun`：挂载目录

7. 效果展示

    ![image-20240109233950183](images/2024-01-09-docker-rclone/image-20240109233950183.png)

## 部署项目 - rclonebrowser

1. 部署 rclonebrowser 项目

    ```yaml
    version: '3.3'
    services:
      rclonebrowser:
        container_name: rclonebrowser
        image: romancin/rclonebrowser:latest
        cap_add:
          - SYS_ADMIN
        devices:
          - /dev/fuse
        security_opt:
          - apparmor=unconfined
        ports:
          - "5801:5800"
          - "5901:5900"
        volumes:
          - /volume1/docker/rclonebrowser-aliyunpan/config:/config
          - /volume2/video/aliyunpan:/media:shared
        environment:
          - GROUP_ID=0
          - USER_ID=0
          - TZ=Asia/Shanghai
          - VNC_PASSWORD=123456
          - ENABLE_CJK_FONT=1
        restart: always
    ```

    > Docker Hub 项目地址：[romancin/rclonebrowser](https://hub.docker.com/r/romancin/rclonebrowser)
    >
    > 特别说明：rclonebrowser 项目自带 rclone 二进制文件，不用单独下载，但版本较低
    >
    > 浏览器访问地址 IP + 端口号：http://10.0.0.6:5801 
    >
    > `- /volume1/docker/rclonebrowser-aliyunpan/config`：配置文件存放地址，配置后会自动生成
    >
    > `- /volume2/video/aliyunpan:/media`：- `<本地挂载目录 >`:/`<容器目录>`
    >
    > `- VNC_PASSWORD`：浏览器访问密码
    >
    > `ENABLE_CJK_FONT`：中文界面需设置参数为 `1` 

2. 浏览器登录

    ![Snipaste_2024-01-10_00-01-27](images/2024-01-09-docker-rclone/Snipaste_2024-01-10_00-01-27.png)

3. 设置参数

    ![Snipaste_2024-01-09_21-22-32](images/2024-01-09-docker-rclone/Snipaste_2024-01-09_21-22-32.png)
    
    ![Snipaste_2024-01-09_21-22-50](images/2024-01-09-docker-rclone/Snipaste_2024-01-09_21-22-50.png)
    
    > 具体配置步骤与 rclone 一致

4. 挂载网盘

    ![Snipaste_2024-01-09_21-23-42](images/2024-01-09-docker-rclone/Snipaste_2024-01-09_21-23-42.png)

    ![Snipaste_2024-01-09_21-24-07](images/2024-01-09-docker-rclone/Snipaste_2024-01-09_21-24-07.png)

    ![Snipaste_2024-01-09_21-25-51](images/2024-01-09-docker-rclone/Snipaste_2024-01-09_21-25-51.png)

    ![Snipaste_2024-01-09_21-26-21](images/2024-01-09-docker-rclone/Snipaste_2024-01-09_21-26-21.png)

    > `/media`：容器目录，与 `- /volume2/video/aliyunpan:/media` 保持一致
    >
    > 说明：容器每次重启都需要重新挂载，暂时未找到解决办法，rclone 项目则不会

5. 成功挂载

    ![Snipaste_2024-01-09_21-27-13](images/2024-01-09-docker-rclone/Snipaste_2024-01-09_21-27-13.png)

6. 调整容器执行参数

    ![Snipaste_2024-01-09_21-28-02](images/2024-01-09-docker-rclone/Snipaste_2024-01-09_21-28-02.png)
    
    ![Snipaste_2024-01-09_21-47-51](images/2024-01-09-docker-rclone/Snipaste_2024-01-09_21-47-51.png)
    
    > 参数修改为：`--allow-other --allow-non-empty --vfs-cache-mode writes`

## 群晖设置

1. 创建自定义脚本

    ![Snipaste_2024-01-09_21-10-23](images/2024-01-09-docker-rclone/Snipaste_2024-01-09_21-10-23.png)
    
    > 说明：是一个 `mount` 命令，它的目的是将 `/volume2` 目录标记为共享的文件系统，否则重启后会报错，挂载失效
    >
    > 参见：[gqbre/docker-rclone-proxy](https://hub.docker.com/r/gqbre/docker-rclone-proxy)
    >
    > 脚本设置：`计划任务` - `新增/触发的任务/用户定义的脚本` - `用户账号：root` - `事件：开机` - `任务设置：mount --make-shared /volume2`

## 参考文档

[Install (rclone.org)](https://rclone.org/install/)

[[新版教程] 阿里云盘通过Docker挂载本地WebDAV实现全自动上传/下载 Rclone挂载本地_阿里云盘webdav最新版-CSDN博客](https://blog.csdn.net/u013659623/article/details/131113516)

[Rclone 使用教程 - 清北博客 (tsinbei.com)](https://blog.tsinbei.com/archives/1445/)
