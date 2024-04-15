---
title: 搭建自己的博客
author: J Song
date: '2024-02-22'
slug: []
categories: []
tags: []
---

这里是我自己建立这个博客小站的一些经验。从零开始摸索，前期尝试了几种搭建网站的方法，觉得还是用 blogdown 比较新手友好，在这里推荐给大家。[blogdown](https://bookdown.org/yihui/blogdown/get-started.html) 是[谢益辉](https://yihui.org)为搭建和维护个人博客网站所写的一个 R 包，感兴趣的童鞋还可以去参观一下大神的个人博客。以下内容主要参考庄亮亮的[帖子](https://cosx.org/2022/03/build-blog-step-by-step/)。

### 需要安装: 
- [R studio](https://posit.co/download/rstudio-desktop/)：在这里面写代码进行网页创建的具体操作

- [GitHub desktop](https://desktop.github.com)：在本地创建网站相关文件之后， 需要 push 到 GitHub 上进行网站部署。用 GitHub desktop 比较方便

- [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

- [Hugo](https://www.gohugo.org/doc/tutorials/installing-on-windows/)：也可在 Rstudio 中安装 blogdown 包之后用`blogdown:install_hugo()`下载

- [R packages:blogdown](https://bookdown.org/yihui/blogdown/get-started.html)

### 网页搭建

在 R studio 里面新建一个 project（也可以不建，但是用 project 来管理比较方便）。用 `install.packages('blogdown')` 安装好 blogdown 包之后，用 `blogdown::build_site()` 创建网站所需要的基本框架文件。如果有自己比较喜欢的主题风格，可以把主题包下载到 themes 目录下面，然后用 `blogdown::build_site(theme = themes/<theme folder>)` 进行网站创建。有关主题的使用在 blogdown 的网页上有详细介绍。

### 创建日志

在 R studio 中用 `blogdown::new_post()` 可以创建新的日志文件。文件会以一个文件夹的形式进行创建，文件夹中包含 index.md文件，之后就可以在这个文件中进行日志内容的编辑。创建的时候可以输入标题名称，但是标题中包含的中文字符貌似不会在文件夹名称中显示。所以一般我的文件夹名称会使用英文名，之后在 index.md 文件中再修改成中文 title。 

### 字体更改

一般主题包中会有自带的字体设置文件，位置在主题包文件夹中的 static/css/ 文件夹中。不过通常这些主题包会进行更新，如果在本地修改过这个文件夹中的内容，后面想要使用更新后的主题包时就需要重新修改，会比较麻烦。因此另一个做法是在主目录下的 static 文件夹中创建 static/css/*.css 文件，这个文件夹的优先级会高于 theme 文件架中的 .css 文件。更改字体可以在 font.css 文件中进行更改。


### 分栏

在主目录下的 config.yaml 文件中进行页面分栏的设置。我是把页面分成 Home, Tech, Post 等几页，所以在 config.yaml 文件中添加下面一段内容：
``` css
menu:
  main:
    - identifier: search
      name: 🔍search
      url: search
      weight: 1
    - identifier: home
      name: Home
      url: /
      weight: 2
    - identifier: posts
      name: Post
      url: /post/
      weight: 3
    - identifier: tech
      name: Tech
      url: /Tech/
      weight: 5
    - identifier: reading
      name: Reading
      url: /Reading/
      weight: 6
    - identifier: lists
      name: Lists
      url: /lists/
      weight: 7
    - name: About
      url: /about/
    - name: GitHub
      url: 'https://github.com/JennySong924/blogdown-myblog'
```

### 页面排版

有关页面排版的内容，例如标题位置、文字边距、字体颜色和大小、目录样式等等都可以在 static/css/custom.css 中进行设置。例如，想要让字数统计靠右对齐，可以这样写：
``` css
.stats {
  text-align: right;
}
```
有关 html 中的各分类器、元素进行了解，可以参考[这里](https://www.w3school.com.cn/tags/html_ref_byfunc.asp)，css 样式相关的内容可以参考[这里](https://www.w3school.com.cn/cssref/index.asp)。


### 杂七杂八

1. 行内数学公式在两个 `$` 符号外面要用 \` 包起来，例如 `$a^2+b^2$` 要这么写：
``` markdown
`$a^2+b^2$`
```


2. 表格内换行可以在行后加 `<br>`





