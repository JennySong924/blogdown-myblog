---
title: 搭建自己的博客
author: J Song
date: '2024-02-22'
slug: []
categories: []
tags: []
---

这里是我自己建立这个博客小站的一些经验。从零开始摸索，前期尝试了几种搭建网站的方法，觉得还是用 blogdown 比较新手友好，在这里推荐给大家。[blogdown](https://bookdown.org/yihui/blogdown/get-started.html) 是[谢益辉](https://yihui.org)为搭建和维护个人博客网站所写的一个 R 包，感兴趣的童鞋还可以去参观一下大神的个人博客。

### 需要安装: 
- [R studio](https://posit.co/download/rstudio-desktop/)：在这里面写代码进行网页创建的具体操作

- [GitHub desktop](https://desktop.github.com)：在本地创建网站相关文件之后， 需要 push 到 GitHub 上进行网站部署。用 GitHub desktop 比较方便

- [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

- [Hugo](https://www.gohugo.org/doc/tutorials/installing-on-windows/)：也可在 Rstudio 中安装 blogdown 包之后用`blogdown:install_hugo()`下载

- [R packages:blogdown](https://bookdown.org/yihui/blogdown/get-started.html)

### 网页搭建

`blogdown::build_site()`

### 创建日志

`blogdown::new_post()`

### 字体更改



### 分栏

### 页面排版

### logo 添加

### 目录排版
