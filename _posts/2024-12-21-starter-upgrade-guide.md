---
title: Starter升级指南
author: yongren
date: 2024-12-21 18:03:06 +0800
categories: [文档, Jekyll]
tags: [jekyll, chirpy, starter]
pin: false
---

## 从 Starter 升级

如果您的网站是使用 [`chirpy-starter`](https://github.com/cotes2020/chirpy-starter) 创建的，请按照以下步骤更新您的仓库。

### 准备工作

本节中提到的操作只需要在将仓库克隆到本地后执行一次。

### 添加 Upstream

执行以下命令添加 upstream 仓库：

```bash
git remote add chirpy https://github.com/cotes2020/chirpy-starter.git
```

验证远程仓库是否成功添加：

```bash
git remote -v
```

输出应包含：

```
chirpy  https://github.com/cotes2020/chirpy-starter.git (fetch)
chirpy  https://github.com/cotes2020/chirpy-starter.git (push)
```

### 处理子模块

如果您的工作流不需要使用子模块 `assets/lib` ，可以执行以下命令：

```bash
git config submodule.assets/lib.ignore all
```

这样可以防止子模块拉取文件并占用本地磁盘空间。

### 获取更新

以下步骤需要每次更新时执行。

1. 从 upstream 获取所有标签：

   ```bash
   git fetch chirpy --tags
   ```

2. M 将 upstream 的最新标签合并到您的本地分支。例如，要合并 `vX.Y.Z` 标签，执行：

   ```bash
   git merge vX.Y.Z --squash --allow-unrelated-histories
   ```

3. 如果冲突文件列表中包含子模块 `assets/lib`:

   - 如果您不打算使用该子模块，执行：

     ```bash
     git restore --staged assets/lib
     ```

   - 如果您已经初始化并启用了子模块，请按照提示操作：

     ```bash
     hint:  - go to submodule (assets/lib), and either merge commit <submodule_commit_hash>
     ```

     记录子模块的 `submodule_commit_hash` ，并更新本地子模块：

     ```bash
     git -C assets/lib merge <submodule_commit_hash>
     ```

4. 手动解决任何剩余的文件冲突。如果需要，保留您的自定义修改。

5. 解决冲突后，创建一个新提交来保存此次升级：

   ```bash
   git add . && git commit -m "chore: upgrade to X.Y.Z" && git push
   ```

6. 更新本地主题 gem：

   ```bash
   bundle update
   ```

## 升级 Fork

如果您是从源项目进行 Fork 的（您的网站上会有源项目的链接），请将最新上游标签 [latest upstream tags](https://github.com/cotes2020/jekyll-theme-chirpy/tags) 合并到您的 Jekyll 网站中以完成升级。此合并可能会与您本地的修改产生冲突，解决这些冲突时要耐心小心。`gemspec` `Gemfile`

自 [`v5.6.0`](https://github.com/cotes2020/jekyll-theme-chirpy/releases/tag/v5.6.0)  版本起，JS 发布文件已被移除，自 [`v7.0.0`](https://github.com/cotes2020/jekyll-theme-chirpy/releases/tag/v7.0.0)  版本起，Bootstrap 的 CSS 变得更加精简。未来的升级中，请自行编译 CSS/JS 文件：

```bash
npm run build
```

然后将它们添加到您的仓库中：

```bash
git add assets/js/dist _sass/vendors -f
```

## 主要版本升级

在升级到主要版本时，请阅读该版本的[发布说明](https://github.com/cotes2020/jekyll-theme-chirpy/releases)，了解项目配置中可能发生的任何变化。

[📝 Helps improve this page](https://github.com/cotes2020/jekyll-theme-chirpy/discussions/new?category=general)

## 参考文档

[Upgrade Guide · cotes2020/jekyll-theme-chirpy Wiki](https://github.com/cotes2020/jekyll-theme-chirpy/wiki/Upgrade-Guide)

