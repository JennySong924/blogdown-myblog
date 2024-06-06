---
title: hugo-Next 主题样式调整
author: J Song
date: '2024-05-28'
slug: []
categories:
  - 技术文章
  - 网络
description: 2024-05-28-hugo-next
keywords: 2024,05,28,hugo,next
lastmod: '2024-05-30T18:30:45+08:00'
---

### code block 样式调整
`./config.yaml` 中更改 `markup -- highlight -- style`

### 页面统计数据中最后更新时间设置
在 `./themes/hugo-theme-next/layouts/partials/_funs/cal_siteinfo.html`中，将第 10 行的`.Date`改为`.Lastmod`，可以将侧边栏网页资讯中的最后更新时间改为文章中的“lastmod”时间。注意文章中的“lastmod”仍然需要在写文章时手动输入。

### 置顶文章
在文章头里加这两个字段
```
toc: true
weight: 1
```

### 行内数学公式渲染
不要用 `$...$`，用`\\( ... \\)`


### 待办：
- 左上角 blog 名称链接页面更改为主页
- code block 折叠
- block quote 格式字体更改