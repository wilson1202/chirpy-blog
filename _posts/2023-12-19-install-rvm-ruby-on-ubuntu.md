---
title: WSL2环境下Ubuntu安装RVM和Ruby
author: yongren
date: 2023-12-19 02:02:01 +0800
categories: [七零八落的折腾, Jekyll]
tags: [jekyll, rvm, ruby, wsl2, ubuntu]
pin: false
image:
  path: /images/2023-12-19-install-rvm-ruby-on-ubuntu/202312190221968.png
typora-root-url: ./
---

## wsl2安装RVM

在Ubuntu中执行上述操作的具体步骤如下：

1. 打开终端：你可以通过按下`Ctrl` + `Alt` + `T` 键盘快捷键来打开终端，或者从应用程序菜单中找到“终端”并打开它。

2. 执行以下命令安装 RVM：

   ```
   \curl -sSL https://get.rvm.io | bash -s stable
   ```

   这个命令使用 `curl` 下载 RVM 安装脚本，并通过管道 `|` 将其传递给 `bash` 执行。

3. 等待安装完成：RVM 的安装过程会下载相应的文件并进行设置，可能需要一些时间。

4. 添加用户到 'rvm' 用户组：

   ```
   sudo usermod -aG rvm your_username
   ```

   将 `your_username` 替换为你的实际用户名。

5. 登出并重新登录：为了应用用户组更改，你需要注销当前用户并重新登录。

   ```
   su root
   su your_username
   ```

6. 执行以下命令开始使用 RVM：

   ```
   source /etc/profile.d/rvm.sh
   ```

   这会启用 RVM。你可能需要在每个新的终端窗口中运行此命令，或者将其添加到你的 shell 配置文件中（例如，`~/.bashrc` 或 `~/.zshrc`）以自动加载。

7. 确保 RVM 已正确安装：可以使用以下命令检查 RVM 版本：

   ```
   rvm --version
   ```

   如果一切顺利，你将看到 RVM 的版本信息。

8. 如果你希望捐赠以支持 RVM 开发和维护，可以访问提供的链接。

请注意，操作系统和软件包管理工具的更新可能会导致某些步骤的变化，建议在执行任何系统级别的更改之前查看官方文档或更新说明。

## 安装 Ruby

```
rvm install ruby-3.2.2
rvm list known   #查看
rvm list   #查看
rvm use 3.2.2 --default   #使用 3.2.2 为默认版本的
ruby -v   #ruby 3.2.2
```

## 参考文档

[尝试用 WSL2 + Ubuntu 开发  Azhubaby Blog](https://blog.azhubaby.com/2023/03/02/2023-03-02-尝试用WSL2+Ubuntu开发/)
