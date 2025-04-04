---
title: VsCode连接Github上传笔记
author: yongren
date: 2024-01-02 00:12:06 +0800
categories: [七零八落的折腾, Jekyll]
tags: [jekyll, chirpy, vscode, github]
pin: false
image:
  path: images/2024-01-01-vscode-connects-to-github-to-upload-notes/0000-2.png
---

## 软件安装

1. 安装 Git

    > Git具体安装方法见：[Git的下载安装-CSDN博客](https://blog.csdn.net/2301_80864686/article/details/134207618?spm=1001.2014.3001.5501)

2. 安装 VsCode

    > VsCode 下载地址：[VSCode下载与安装使用教程【超详细讲解】-CSDN博客](https://blog.csdn.net/m0_67906358/article/details/129302022)

## Git 设置

1. Git 配置（首次安装配置）

    ```
    # git全局配置指令
    git config --global user.name "your_name"
    git config --global user.email  "your_email"

    # 查看全局配置
    git config --global --list

    # 删除全局配置的用户名
    git config --global --unset user.name

    # 删除全局配置的邮箱
    git config --global --unset user.email
    ```

2. ssh 设置（连接github正常则跳过）

    ```
    # 生成SSH Keys，默认会生成id_rsa、id_rsa.pub文件，分别为私钥、公钥
    ssh-keygen -t rsa -C "...@qq.com"
    
    # ssh-agent默认只会读取id_rsa文件，需要将私钥添加到ssh-agent中
    # 如果报错：unabel to start ssh-agent service，error：1058，需要开启OpenSSH Authentication Agent服务
    #（计算机管理-->服务-->OpenSSH Authentication Agent）
    ssh-add ./id_rsa_github
    
    # 配置config文件
    
    # Default github 
    Host github.com
       HostName github.com
       User "<user_name>"
       IdentityFile ~/.ssh/id_rsa_github
    
    # gitee
    Host gitee.com
       HostName gitee.com
       User "<user_name>"
       IdentityFile ~/.ssh/id_rsa_gitee
    
    # 添加公钥到Git系统，测试
    ssh -T git@github.com
    ```

## 链接 GitHub 仓库

1. 打开克隆快捷键，`ctrl+shift+P`

    ![Snipaste_2024-01-01_23-41-45](/images/2024-01-01-vscode-connects-to-github-to-upload-notes/Snipaste_2024-01-01_23-41-45.png)

2. 根据提示选择仓库

    ![Snipaste_2024-01-01_23-40-33](/images/2024-01-01-vscode-connects-to-github-to-upload-notes/Snipaste_2024-01-01_23-40-33.png)

3. 选择本地存储位置

    ![Snipaste_2024-01-01_23-49-10](/images/2024-01-01-vscode-connects-to-github-to-upload-notes/Snipaste_2024-01-01_23-49-10.png)

## 提交代码

1. 交互界面提交
    ![image-20240101235709086](/images/2024-01-01-vscode-connects-to-github-to-upload-notes/image-20240101235709086.png)

2. 终端提交，与在 Windows 终端提几乎一致

    ![image-20240102000259439](/images/2024-01-01-vscode-connects-to-github-to-upload-notes/image-20240102000259439.png)

    > 首次在交互界面提交可能会提示权限问题，类似于以下
    >
    > ```
    > You don't have permissions to push to "wilson1202/wilson1202-images" on GitHubWould you like to create a fork and push to it instead?
    > ```
    >
    > 猜测是因为连接克隆到 github 仓库这一步，使用的是 https 链接，尝试切换至 ssh 链接
    >
    > ```
    >  git remote -v   #查看当前远程仓库的详细信息
    >  git remote remove origin   #移除当前远程仓库关联
    >  git remote add origin git@github.com:username/username.github.io.git   #添加远程仓库关联
    > ```

## 参考文档

[VsCode使用Git上传代码至GitHub上_github vscode提交-CSDN博客](https://blog.csdn.net/2301_80864686/article/details/134207692) 

[VSCode Github - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/464794757)
