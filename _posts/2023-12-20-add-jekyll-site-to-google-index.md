---
title: Jekyll站点添加至谷歌收录
author: yongren
date: 2023-12-20 09:02:12 +0800
categories: [七零八落的折腾, Jekyll]
tags: [jekyll, chirpy, google, scarch, console, verification, site, map, analytics, id]
pin: false
image:
  path: /images/2023-12-20-add-jekyll-site-to-google-index/202312201042874.png
typora-root-url: ./
---

Jekyll搭建完毕，并不意味着可以通过Google或百度在互联网搜索到我们的文章，需要手动提交索引设置，本文记录添加谷歌收录的步骤。

## 前提

1. Jekyll 在 GitHub 部署完毕
2. Jekyll 安装 [chirpy-starter](https://github.com/cotes2020/chirpy-starter) 或 [jekyll-theme-chirpy](https://github.com/cotes2020/jekyll-theme-chirpy) 主题
3. 其他静态博客或主题根据实际情况设置对应参数

## Google 搜索控制台

1. Google 搜索控制台地址

    [Google Search Console](https://search.google.com/search-console/about)

2. 选择资源类型

    ![image-20231220001504675](/images/2023-12-20-add-jekyll-site-to-google-index/202312200015792.png)

    > GitHub Pages 部署选择网址前缀方式，填写地址 `https://username.github.io`	

3. 获取 google_site_verification 码

    ![image-20231220000944532](/images/2023-12-20-add-jekyll-site-to-google-index/202312200009659.png)

    ```
    <meta name="google-site-verification" content="glthvg..........nJjY" />
    ```

    > 保存 content 值

4. Jekyll 配置文件 _config 添加 google_site_verification 参数

    ![image-20231220100954385](/images/2023-12-20-add-jekyll-site-to-google-index/202312201009420.png)

5. 验证 google_site_verification 参数

    ![google-verification-succeed](/images/2023-12-20-add-jekyll-site-to-google-index/202312200011368.png)

6. 添加站点地图

    站点地图（Site Map）是用来注明网站结构的文件，将网站结构提交搜索引擎，以便于更高效的收录网站内容，快速建立索引。

    - Site Map 制作网站

      [XML Sitemaps](https://www.xml-sitemaps.com/)

      ![image-20231220002839493](/images/2023-12-20-add-jekyll-site-to-google-index/202312200028560.png)

    - 下载 XML 文件放到 GitHub 库根目录

      ![image-20231220003859877](/images/2023-12-20-add-jekyll-site-to-google-index/202312200038963.png)

    - 控制台添加站点地图

      ![image-20231220004553394](/images/2023-12-20-add-jekyll-site-to-google-index/202312200045476.png)

    - 收录检测

      ![image-20231220014413043](/images/2023-12-20-add-jekyll-site-to-google-index/202312200144159.png)
      
      > 谷歌自动收录需要等8-24小时左右，也可以在网址检查页面做手动提交
    

##  Google Analytics

Google Analytics（分析）可以为用户提供更详细的站点访问统计等数据，有SEO需求的强烈推荐，没有需求可以跳过。

1. Google Analytics

    [Google Analytics（分析）](https://analytics.google.com)

2. 注册过程

    ![Snipaste_2023-12-19_19-12-11](/images/2023-12-20-add-jekyll-site-to-google-index/202312200916651.png)

    ![Snipaste_2023-12-19_19-13-00](/images/2023-12-20-add-jekyll-site-to-google-index/202312200917555.png)

    ![Snipaste_2023-12-19_19-13-19](/images/2023-12-20-add-jekyll-site-to-google-index/202312200918678.png)

    ![Snipaste_2023-12-19_19-14-10](/images/2023-12-20-add-jekyll-site-to-google-index/202312200918926.png)

    ![Snipaste_2023-12-19_19-14-24](/images/2023-12-20-add-jekyll-site-to-google-index/202312200919898.png)

    ![Snipaste_2023-12-19_19-14-41](/images/2023-12-20-add-jekyll-site-to-google-index/202312200919867.png)

    ![Snipaste_2023-12-19_19-18-04](/images/2023-12-20-add-jekyll-site-to-google-index/202312200919087.png)

    ![Snipaste_2023-12-19_19-20-11](/images/2023-12-20-add-jekyll-site-to-google-index/202312200920439.png)

    ![image-20231220093237314](/images/2023-12-20-add-jekyll-site-to-google-index/202312200932445.png)

3. Jekyll 配置文件 _config 添加 Google Analytics ID，并测试验证

    ![image-20231220101134681](/images/2023-12-20-add-jekyll-site-to-google-index/202312201011705.png)
    
    ![image-20231220092416570](/images/2023-12-20-add-jekyll-site-to-google-index/202312200924700.png)

4. 完成数据收集

    ![Snipaste_2023-12-20_09-00-34](/images/2023-12-20-add-jekyll-site-to-google-index/202312200935229.png)

    > 数据收集的过程大概要8-24小时左右

5. 查看流量分析数据

    ![image-20231220093930271](/images/2023-12-20-add-jekyll-site-to-google-index/202312200939410.png)


## 参考文档

[SEO Google 搜索引擎收录设置](https://qlzhu.github.io/blog/10730/)

[让Google搜索到用Jekyll搭建在Github Pages上的博客 - EvanLi的博客](https://evanli.github.io/blog/2018/10/25/let-jekyll-github-pages-be-searched-by-google/)

[为网站和/或应用设置 Google Analytics（分析）- Google Analytics（分析）帮助](https://support.google.com/analytics/answer/9304153?visit_id=638385762624015300-1087070166&rd=2&hl=zh-Hans&sjid=12857746639811216264-AP#zippy=%2C网站%2C使用-google-跟踪代码管理器添加代码)

[Search Console帮助-在 Search Console 中添加网站资源](https://support.google.com/webmasters/answer/34592?hl=zh-Hans&sjid=12857746639811216264-AP)
