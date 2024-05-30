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
![photo](https://private-user-images.githubusercontent.com/32083698/255953234-774248aa-99c8-4f23-9b97-fa1ca7a9e73b.jpg?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MTcwNjQxOTQsIm5iZiI6MTcxNzA2Mzg5NCwicGF0aCI6Ii8zMjA4MzY5OC8yNTU5NTMyMzQtNzc0MjQ4YWEtOTljOC00ZjIzLTliOTctZmExY2E3YTllNzNiLmpwZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDA1MzAlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwNTMwVDEwMTEzNFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTUzY2ZlNGE2NzUzMDRhMjBhNDJlOWUyMzY3N2Q0YTM4MTQ2NDdiNjJjNDQ3OGZlZjc2ODYwMWJhODc5NDNiN2UmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.LbPucYlPd-AoTWM_0qHCBBjtp-In3hRVm1SS1jEUJOw)

### 待办：
- 左上角 blog 名称链接页面更改为主页
- 行内公式渲染
- code block 折叠
- block quote 格式字体更改