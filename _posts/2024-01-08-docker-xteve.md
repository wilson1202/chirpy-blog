---
title: Docker 部署 xTeVe
author: yongren
date: 2024-01-08 15:13:18 +0800
categories: [七零八落的折腾, Docker]
tags: [docker, xteve, ffmpeg, iptv, plex]
pin: false
image:
  path: images/2024-01-08-docker-xteve/docker-xteve.png
---

## 部署简介

1. 部署项目
2. 添加 iptv 组播文件
3. 设置 xteve
4. 设置 ffmpeg 解码
5. 设置 Plex 电视直播

## 部署项目

1. 部署环境：ESXi-8.0，Ubuntu-18.04

2. Docker Hub 项目地址：[alturismo/xteve - Docker Image](https://hub.docker.com/r/alturismo/xteve)

3. docker-compose.yml 部署

    ```yaml
     version: '3.3'
     services:
       xteve:
           container_name: xteve
           image: alturismo/xteve:latest
           network_mode: host
           restart: unless-stopped
           logging:
               options:
                   max-size: 10m
                   max-file: 3
           environment:
               - TZ=Europe/Berlin
           volumes:
               - '/docker/xteve/:/root/.xteve:rw'
               - '/docker/xteve/_config:/config:rw'
               - '/tmp/xteve/:/tmp/xteve:rw'
    ```

## 添加 iptv 组播文件

1. 添加直播/组播文件 `CMCC-IPTV.m3u`

    ![image-20240108170217153](/images/2024-01-08-docker-xteve/image-20240108170217153.png)

    > 路径：`/docker/xteve/_config/CMCC-IPTV.m3u`

## 设置 xteve

1. 打开后台，默认端口号 `34400` 

    > 例如：http://10.0.0.7:34400/web/

2. 设置播放端数量，如果有公网访问需求，需要考虑实际上传带宽

    ![image-2965397799](/images/2024-01-08-docker-xteve/2965397799.png)

3. 添加直播/组播文件 `CMCC-IPTV.m3u`

    > 路径：`/config/CMCC-IPTV.m3u`

4. 添加节目表地址

    > http://epg.51zmt.top:8000/e.xml

5. 完成设置

    ![image-20240108172802798](/images/2024-01-08-docker-xteve/image-20240108172802798.png)

## 设置 ffmpeg 解码

1. 安装 ffmpeg

    ```
    #更新源码
    sudo apt update
    
    #安装ffmpeg
    sudo apt install ffmpeg
    ```

    >  提示需要其他依赖，按照提示进行操作即可
    >
    >  ```
    >  #先安装
    >  sudo apt --fix-broken install
    >  
    >  #再安装
    >  sudo apt install ffmpeg#  
    >  
    >  #或者使用指令
    >  sudo apt install ffmpeg --fix-missing
    >  ```

2. 验证安装结果

    ```
    ffmpeg -version
    ```

    ![image-20240108175020146](/images/2024-01-08-docker-xteve/image-20240108175020146.png)

3. 配置 xTeVe

    ![image-20240108175939666](/images/2024-01-08-docker-xteve/image-20240108175939666.png)

    > 需要修改三处配置（画面播放正常，FFmpeg Options 参数可不调整）
    >
    > - Stream Buffer：`FFmpeg: (FFmpeg connects to the streaming server)`
    > - FFmpeg Binary Path：`/usr/bin/ffmpeg`
    > - FFmpeg Binary Path（群晖7.0）：`/var/packages/ffmpeg6/target/bin/ffmpeg`
    > - FFmpeg Options：`-hide_banner -i [URL] -c:a libmp3lame -vcodec copy -f mpegts pipe:1`

## 设置 Plex 电视直播

1. 添加设备

    ![image-20240108181053443](/images/2024-01-08-docker-xteve/image-20240108181053443.png)

2. 调谐器设置-国家/地区

    ![image-20240108181210196](/images/2024-01-08-docker-xteve/image-20240108181210196.png)

3. 调谐器设置-XMLTV指南

    ![image-20240108181442794](/images/2024-01-08-docker-xteve/image-20240108181442794.png)

    > XMLTV指南：http://10.0.0.3:34400/xmltv/xteve.xml   

4. 自动配置频道

    ![image-20240108181705118](/images/2024-01-08-docker-xteve/image-20240108181705118.png)

5. 完成设置

    ![image-20240108181854558](/images/2024-01-08-docker-xteve/image-20240108181854558.png)

6. 查看指南

    ![image-20240108182057253](/images/2024-01-08-docker-xteve/image-20240108182057253.png)

7. 播放测试

    ![image-20240108182219464](/images/2024-01-08-docker-xteve/image-20240108182219464.png)

## 参考文档

[如何在 Plex 内收看 IPTV 直播电视？ - 哔哩哔哩 (bilibili.com)](https://www.bilibili.com/read/cv22239319/)

[ffmpeg Documentation](https://ffmpeg.org/ffmpeg.html)

[FFmpeg](https://trac.ffmpeg.org/#Encoding)
