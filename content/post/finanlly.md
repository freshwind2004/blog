---
title: "Hugo的云端自动化更新"
date: 2022-10-13T22:00:00+08:00
draft: false
categories: Development
tags: [hugo, actions, github, automation, 自动化]
---

# 前言

所谓Hugo的自动更新，就是抛开本地端，在Github网页端编辑博客内容，透过Github Actions功能就可以自动渲染出静态网页，终于实现了博客“自动化”。

那么博客不应该本来就是自动化的吗？当你在后台输入你的文章，在前台就能显示你的文章。这样的技术，新浪博客不是在十几年前就实现了吗？

的确如此。然而，新的趋势却是抛弃服务端的自动化，回归本地的“格式化”。这么做的意义在于，服务端不再需要进行“多余的”数据库运算，只需要呈现静态的网站内容，以提高站点的整体相应速度。那么具体怎么做呢？很简单，但是也很笨。那就是一旦发生内容的变化，比如把“那天”改成“那一天”，就重新生成一遍所有包含这段文字的全部页面。这些页面包括：内容本体、分类页、标签页、首页等等。幸运的是，我们不用手工完成这些乏味的修订工作，而是依赖于Hugo这种自动化工具。

然而，当我们选择使用Hugo生成器的时候，就必须启用本地端。在本地生成好完整的静态（HTML）文件后，上传至云端。既可笑，又无奈。是的，我们已经从虚拟主机进化到了云端；然而，对于内容本身，我们又回到了那个一页页上传HTML的场景。

好在，Github为我们提供了一种自动化的方案。那就是在云端再进行一次“页面生成”。这一项工作是通过Github Actions完成的。Github Actions根据用户的指令，在云端执行和本地同样的工作。我们就又可以抛弃本地端了。

# 解决问题

**Github**的功能迭代太快。关于使用Github Actions更新Hugo的代码大都没法用。

反复测试了许久，正确的打开方式如下：

1. 进入Repo->Pages选项，将Build and deployment方式更改为“Github Actions”
2. 在新弹出的编辑界面贴上下面的代码

# 代码

```yml
# Sample workflow for building and deploying a Hugo site to GitHub Pages
name: Rebuild Hugo

on:
  # Runs on pushes targeting the default branch
  push:
    branches: ["main"]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v1
      with:
        submodules: true

    - name: Setup Hugo
      uses: peaceiris/actions-hugo@v2
      with:
        hugo-version: '0.102.3'

    - name: Build
      run:
        hugo --minify
        
    - name: Upload artifact
      uses: actions/upload-pages-artifact@v1
      with:
        path: ./public

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v1
```

2022年10月测试通过。时间久了可能就又不通了。

# 然后

当你成功部署了以上Github Actions之后，你需要在./content目录下手工新建.md文档创建新的文章，或者编辑./content目录下已经存在的文章。保存后，Github Actions便会执行一次“生成”作业，使得静态页面得以更新。

# 感想

补充几句。

这段Action指令总算让我明白了Github Pages的底层逻辑。

你创建的Repo只是Repo，即便他是一个Public Repo，它也只是Github小破站上的一个代码库，无论你是否采用handsome.github.io的形式命名。

当你想把Repo的内容发布到公网（变成独立站点），你需要运行一个`deploy-pages`的指令，将Repo内的文件上传至特定的位置。

这样的上传必须在每次更改时进行。实际上Repo是虚拟的本地，这个“特定位置”则是虚拟云端。至于特定位置在哪？我不知道，反正你访问不到。除了从Repo内上传，你根本碰不到。

那么，你不配置Action的时候呢？

实际上一旦你启用Github Pages，系统会默认为你配置符合Jekyll格式静态内容的Action。

这时候，你用md也好html也好，每次同步的文件都会被正常编译，并传至“特定空间”。
