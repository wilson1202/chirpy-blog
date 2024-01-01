---
title: Cloudflare Pages部署Jekyll
author: yongren
date: 2023-12-19 12:32:54 +0800
categories: [七零八落的折腾, Jekyll]
tags: [jekyll, cloudflare, pages]
pin: false
image:
  path: /images/2023-12-19-install-jekyll-on-cloudflare-pages/202312191237181.png
typora-root-url: ./
---

> Cloudflare Pages部署Jekyll主要是作为备站使用

## 注册Cloudflare

1. 步骤

   略过

## 创建Pages

1. 创建应用程序

   ![Snipaste_2023-12-19_12-52-28](/images/2023-12-19-install-jekyll-on-cloudflare-pages/202312191252528.png)

2. 连接到Git

   ![image-20231219125409452](/images/2023-12-19-install-jekyll-on-cloudflare-pages/202312191254505.png)

3. 关联GitHub账户对应的存储库

   ![image-20231219125704437](/images/2023-12-19-install-jekyll-on-cloudflare-pages/202312191257505.png)

4. 部署设置

   项目名称：随意

   生产分支：根据实际情况

   构建设置：

   - 架构预设：勾选`Jekyll`，构建命令和输出目录自动生成
   - 环境变量（高级）：添加`RUBY_VERSION`变量，值根据实际情况
   - 其他不动

   ![image-20231219130509739](/images/2023-12-19-install-jekyll-on-cloudflare-pages/202312191305791.png)

5. 完成部署

   ![image-20231219130819608](/images/2023-12-19-install-jekyll-on-cloudflare-pages/202312191308671.png)

## 参考文档

[Deploy a Jekyll site · Cloudflare Pages docs](https://developers.cloudflare.com/pages/framework-guides/deploy-a-jekyll-site/)
