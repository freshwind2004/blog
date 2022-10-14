---
title: "总算成了 Hi there, Hugo"
date: 2022-10-13T22:50:00+08:00
draft: false
---

> 规矩出的快，改得更快。于是网上的攻略就都失了效。

我不是说_官老爷_朝令夕改。而是**Github**的功能迭代太快。

关于使用Github Actions更新Hugo的代码大都没法用，反复测试了许久，正确的打开方式如下：

1. 进入Repo->Pages选项，将Build and deployment改为“Github Actions”
2. 在新弹出的编辑界面贴上下面的代码

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

补充几句。

这段Action指令总算让我明白了Github Pages的底层逻辑。

你创建的Repo只是Repo，即便他是一个Public Repo，它也只是Github小破站上的一个代码库，无论你是否采用handsome.github.io的形式命名。

当你想把Repo的内容发布到公网（变成独立站点），你需要运行一个`deploy-pages`的指令，将Repo内的文件上传至特定的位置。

这样的上传必须在每次更改时进行。实际上Repo是虚拟的本地，这个“特定位置”则是虚拟云端。至于特定位置在哪？我不知道，反正你访问不到。除了从Repo内上传，你根本碰不到。

那么，你不配置Action的时候呢？

实际上一旦你启用Github Pages，系统会默认为你配置符合Jekyll格式上传的Action。

这时候，你用md也好html也好，每次同步的文件都会被正常编译，并传至“特定空间”。
