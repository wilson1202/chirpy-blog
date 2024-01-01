---
title: Jekyll添加giscus评论系统
author: yongren
date: 2023-12-19 15:46:10 +0800
categories: [七零八落的折腾, Jekyll]
tags: [jekyll, giscus, chirpy]
pin: false
image:
  path: /images/2023-12-19-install-giscus-on-jekyll/202312191550055.png
typora-root-url: ./
---

## 准备仓库

1. 可以选择博客所在的 `username.github.io` 仓库
2. 也可以为评论系统单独创建一个仓库

## 添加 giscus 

1. 安装 giscus 

    [GitHub Apps - giscus](https://github.com/apps/giscus)
    ![image-20231219160516842](/images/2023-12-19-install-giscus-on-jekyll/202312191605888.png)

2. 选择仓库

    ![image-20231219173549559](/images/2023-12-19-install-giscus-on-jekyll/202312191735600.png)

3. 开启 Discussion

    ![image-20231219161153383](/images/2023-12-19-install-giscus-on-jekyll/202312191611411.png)

    > 路径： `Setings` - `General` - `Features` - `Discussions `

4. giscus 配置

    语言：简体中文

    仓库： `username/username-comments` 

    页面 ↔️ discussion 映射关系： Discussion 的标题包含页面的 `pathname` 

    Discussion 分类：Announcements

    > 一般选择 Announcements，因为 Announcements 类型的 discussion 只有管理员才有权限操作，这样便于管理

5. 查看 giscus 配置

    页面划到底部

    ```
    <script src="https://giscus.app/client.js"
           data-repo="username/username-comments"
           data-repo-id="R_sd000001"
           data-category="Announcements"
           data-category-id="DIC_sd000001dfdf"
           data-mapping="pathname"
           data-strict="0"
           data-reactions-enabled="1"
           data-emit-metadata="0"
           data-input-position="bottom"
           data-theme="preferred_color_scheme"
           data-lang="zh-CN"
           crossorigin="anonymous"
           async>
    </script>
    ```

6. 配置到 Jekyll 

    配置主题：[cotes2020/jekyll-theme-chirpy](https://github.com/cotes2020/jekyll-theme-chirpy)

    ```
    comments:
     active: giscus # The global switch for posts comments, e.g., 'disqus'.  Keep it empty means disable
     # The active options are as follows:
     giscus:
       repo: "username/username-comments" # <gh-username>/<repo>
       repo_id: "R_sd000001"
       category: "Announcements"
       category_id: "DIC_sd000001dfdf"
       mapping: "pathname" # optional, default to 'pathname'
       input_position: "bottom" # optional, default to 'bottom'
       lang: "zh-CN" # optional, default to the value of `site.lang`
       reactions_enabled: # optional, default to the value of `1`
    ```

7. 效果展示

    ![image-20231219163018886](/images/2023-12-19-install-giscus-on-jekyll/202312191630929.png)

## 参考文档

[Hugo 博客引入 Giscus 评论系统 - (lixueduan.com)](https://www.lixueduan.com/posts/blog/02-add-giscus-comment/)

[使用Jekyll + GitHub Pages搭建个人博客-CSDN博客](https://blog.csdn.net/zzy979481894/article/details/132678717)