---
title: "图形可视化"
subtitle: ""
date: 2022-10-27T17:54:00+08:00
draft: false
author: ""
authorLink: ""
authorEmail: ""
description: ""
keywords: ""
weight: 0

tags:
- Qt
- Python
- 可视化
- GUI
categories:
- Development

hiddenFromHomePage: false
hiddenFromSearch: false

summary: ""
resources:
- name: featured-image
  src: featured-image.jpg
- name: featured-image-preview
  src: featured-image-preview.jpg

---

# Qt from Nokia

Qt 居然源自于诺基亚。当然，后来Nokia明智的将Qt出售给了。

> Qt最早是1991年由挪威的 Eirik Chambe-Eng 和 Haavard Nord 开发的， 他们随后于1994年3月4号正式成立奇趣科技公司（Trolltech）。Qt原本是商业授权的跨平台开发库， 在2000年奇趣科技公司为开源社区发布了遵循 GPL（GNU General Public License）许可证的开源版本。 在2008年，诺基亚公司收购了奇趣科技公司，并增加了 LGPL（GNU Lesser General Public License）的授权模式。 诺基亚联合英特尔利用 Qt 开发了全新的智能手机系统 MeeGo，可惜遭遇了微软木马屠城，诺基亚被迫放弃了 MeeGo， 而 Qt 商业授权业务也于2011年3月出售给了芬兰IT服务公司 Digia。当然好消息是 Digia 于2014年9月宣布成立 Qt Company 全资子公司，独立运营 Qt 商业授权业务。

# Qt Binding

> Qt 是纯 C++ 开发的，所以学好 C++ 比较有必要。Qt 还存在 Python、Ruby、Perl 等脚本语言的绑定， 也就是说可以使用脚本语言开发基于 Qt 的程序。

Qt本身是一个C++程序，它会在后台处理“绑定”程序提交的GUI业务，呈现到桌面。也就是说，Python是Qt的“指令语言”；Qt相对独立于Python运行，只负责窗体相关的事务。

# Qt for Python

Qt在python上有两个分支。其中一个是pyqt，另一个是pyside；二者加起来一共有五六个版本。并不互相兼容，经常做一些使用习惯上的小改变，annoying。学习资料也不全面。
