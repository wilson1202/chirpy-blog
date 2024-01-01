---
title: WSL2环境下Ubuntu部署Hexo博客
author: yongren
date: 2023-12-16 14:05:50 +0800
categories: [七零八落的折腾, Hexo]
tags: [hexo, nvm, npm, node, wsl2, ubuntu]
pin: false
image:
  path: /images/2023-12-16-install-hexo-on-wsl2-ubuntu/202312171111513.png
typora-root-url: ./
---

> 转载：[博客1:wsl2上配置hexo博客-CSDN博客](https://blog.csdn.net/iamtheplayer/article/details/128317770)

## 工作环境

Windows下的[wsl2](https://so.csdn.net/so/search?q=wsl2&spm=1001.2101.3001.7020)，Ubuntu-20.04

1. 安装[nvm](https://so.csdn.net/so/search?q=nvm&spm=1001.2101.3001.7020)(网络不行的可以参考文献1)

    ```
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
    
    ###二选一###
    wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
    ```

    进入.nvm目录，通过install.sh来安装

    ```
    cd .nvm
    bash install.sh
    ```

    切换到root目录下（返回上层目录），更新.bashrc

    ```
    source .bashrc
    ```

2. 安装Node.js

    ```
    nvm install --lts
    ```

3. 检查nvm和Node.js是否安装成功

    ```
    node -v
    nvm -v
    ```

    > 效果是显示node.js和nvm的版本

4. 安装npm的镜像源

    ```
    npm install -g cnpm --rigistry=https://registry.npm.taobao.org
    ```

## 部署hexo
1. 安装hexo
    ```
    cnpm install -g hexo-cli
    ```

6. 检查hexo是否安装成功

    ```
    hexo -v
    ```

7. 部署hexo

    ```
    mkdir 文件夹
    cd 文件夹
    hexo init
    ```

    > - 错误1
    >
    >   `Failed to install dependencies. Please run ...`
    >
    > - 解决方案
    >
    >   `cnpm install`
    
8. 开启hexo

    ```
    hexo s
    ```

    现在hexo就已经开启了

    shell会显示端口

    ```
    http://localhost:端口号（默认4000）/
    ```

    在浏览器中输入上面的网址，即可进入博客网页。

    > - 错误2
    >
    >   网页显示无法访问
    >
    > - 原因
    >
    >   端口被占用
    >
    > - 解决方案
    >
    >   将开启hexo的命令改为
    >
    >   `hexo s -p 其他的端口号`
    >
    
    可以通过Ctrl+C来关闭这一开启

## 博客相关
1. 新建博客

    ```
    hexo n "博客名字"
    ```

    可以通过它显示的路径找到博客的具体文件

    文件名是`博客名字.md`

    里面使用`Markdown`格式写

2. 更新博客

    清除原本的博客网页内容

    ```
    hexo clean
    ```

    生成新的博客网页内容

    ```
    hexo g
    ```

    然后再开启一次hexo即可

3. 更改博客主题

    进入到`blog/themes/`里，在网上搜索你喜欢的主题的github地址，`git clone`到这个目录下来，将克隆的文件夹名填写到`blog/_config.yml`里的`theme`一栏

    再更新即可

    > - 错误3
    >
    >   缺包
    >
    > - 解决方案
    >
    >   `cnpm install 包名@版本`
    >
    > - 错误4
    >
    >   __不起作用
    >
    > - 解决方案
    >
    >   改为**

## 部署到网络上

1. 安装deployer

   登录自己的github账号，`new repository`，`repository name`必须为`用户名.github.io`

   接下来在bash里安装deployer

   `cnpm install --save hexo-deployer-git`

2. 更改配置

   进入`blog/`，将`_config.yml`的最后一部分的`deploy`改为

   `deploy:`

   `type: git`（一定要有空格）

   `repo: 刚刚的仓库地址`

   `branch: master`

3. 上传

   `hexo d`

   输入你的github用户名并在密码部分输入你的秘钥（可以在github的setting里的developer setting生成）

   在浏览器中打开`用户名.github.io`

## 参考文档

1. [Ubuntu（WSL）中Node.js环境安装_ZHOUZAIHUI的博客-CSDN博客_wsl 安装node](https://blog.csdn.net/gandongusa/article/details/123010941)
2. [手把手教你从0开始搭建自己的个人博客 hexo_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Yb411a7ty/?vd_source=92b6f4a65908f6aa7ab090b39f23b212)
3. [如何解决安装hexo init blog 时，出现WARN Failed to install dependencies. Please run ‘npm install‘ in “E:\tests_简朴-ocean的博客-CSDN博客](https://blog.csdn.net/weixin_44237337/article/details/119994618)
4. [[Bug0014\] Hexo+Github 搭建博客报错 ：remote: Support for password authentication was removed on August 13, 202x. Please use a personal access token instead. - Code7Rain - 博客园 (cnblogs.com)](https://www.cnblogs.com/Code-Rain/p/16357616.html)
