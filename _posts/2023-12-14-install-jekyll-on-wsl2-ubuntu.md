---
title: WSL2环境下Ubuntu安装Jekyll
author: yongren
date: 2023-12-14 22:00:10 +0800
categories: [七零八落的折腾, Jekyll]
tags: [jekyll, ruby, rvm, gems, npm, bundler, wsl2, ubuntu]
pin: false
image:
  path: /images/2023-12-14-install-jekyll-on-wsl2-ubuntu/202312171107359.png
typora-root-url: ./
---

## 安装依赖

1. 安装 Ruby （官方推荐方式）

   安装依赖项[永久链接](https://jekyllrb.com/docs/installation/ubuntu/#install-dependencies)

   安装 Ruby 和其他[先决条件](https://jekyllrb.com/docs/installation/#requirements)：

   ```
   sudo apt-get install ruby-full build-essential zlib1g-dev
   ```

   > RVM方式（二选一），详见：[WSL2环境下Ubuntu安装RVM和Ruby](https://wilson1202.github.io/posts/install-rvm-ruby-on-ubuntu/)
   >

2. 安装 RubyGems 包

    避免以 root 用户身份安装 RubyGems 包（称为 gems）。相反，为您的用户帐户设置一个 gem 安装目录。以下命令会将环境变量添加到您的`~/.bashrc`文件中以配置gem安装路径：

    ```
    echo '# Install Ruby Gems to ~/gems' >> ~/.bashrc
    echo 'export GEM_HOME="$HOME/gems"' >> ~/.bashrc
    echo 'export PATH="$HOME/gems/bin:$PATH"' >> ~/.bashrc
    source ~/.bashrc
    ```

## 安装 Jekyll 和 Bundler

1. 官方

   最后，安装 Jekyll 和 Bundler（此步为官方，直接看下面安装）

    ```
    gem install jekyll bundler
    ```

    就是这样！您已准备好开始使用 Jekyll。

2. 自定义

    ```
    git clone https://github.com/cotes2020/jekyll-theme-chirpy.git   #拉去代码
    cd jekyll-theme-chirpy   #进入目录
    ruby --version   #查看ruby版本
    bundle install   #安装bundle
    jekyll s   #运行jekyll，默认端口号4000，自定义端口号 --port 4001
    ```
    
    > npm i && npm run build   #安装npm
    >
    > bundle exec jekyll s   #使用项目中指定的 Gem 版本

## Github推送文章

1. 创建公钥私钥对

   创建公钥私钥对的代码，其中 rsa 是一个非对称密钥算法：

   ```
   ssh-keygen -t rsa
   ```

    查看生成的公钥文件：

    ```
    cat ~/.ssh/id_rsa.pub
    ```

2. 关联远程仓库

    ```
    git remote -v   #查看当前远程仓库的详细信息
    git remote remove origin   #移除当前远程仓库关联
    git remote add origin git@github.com:username/username.github.io.git   #添加远程仓库关联
    ```


3. GitHub Pages设置

    ![202312191219877](/images/2023-12-14-install-jekyll-on-wsl2-ubuntu/202312191219877.png)

    > 设置路径：`Setings`-`Pages`-`Build and deployment`-`Source`-`GitHub Actions`
    >
    > 该设置仅针对chirpy主题，其他主题请阅读对应的部署说明

4. 推送文章

    ```
    git status   #查看工作目录的状态
    git add <file>   #将更改的工作目录添加到暂存区，准备提交/git rm <file>
    git commit -m "add <file>"   #描述此次提交的内容/git commit -m "rm <file>"
    git push -u origin master   #第一次将本地仓库推送到远程仓库master分支
    git push   #将本地仓库的更改同步到远程仓库
    ```



## 参考文档

[Jekyll on Ubuntu (jekyllrb.com)](https://jekyllrb.com/docs/installation/ubuntu/)

[使用 Jekyll 部署 GitHub Pages_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1C14y187Nh/?spm_id_from=333.337.search-card.all.click&vd_source=429a3471dab07d1f8a77684b3a2ffe13)

[GitHub（七）远程仓库：远程仓库的创建、配置 SSH 公钥 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/654126203)

[Open Source-HTML 用于呈现专业写作的最小、响应迅速且功能强大的 Jekyll 主题。-糯米PHP (nuomiphp.com)](https://www.nuomiphp.com/github/zh/6294db9fba76ce5b7519b18e.html)
