---
title: '[转] 自定义短语示例'
author: J Song
date: '2024-05-17'
slug: []
categories:
  - 技术文章
  - 网络
tags: []
_build:
  render: never
  list: never
  publishResources: no
---

之前引用一些文章内容的时候用 quote，格式不太满意，自己暂时没找到自定义 quote 格式的方法（在 static 文件夹里面添加 css 文件之后没啥效果。。。不确定是自己写的 code 的问题还是不能用 css 文件来做）。然后在示例网站发现了作者提供的一种方法，收藏一下 [Hugo-next theme 作者提供的内容](https://hugo-next.eu.org/demo/shortcodes.html#more)。

---

虽然 `Markdown` 语法已经非常丰富能够满足我们写文章的绝大部分需求，但是为更好的对文章内容进行更友好的排版，为此设计了一套自定义的短语，便于在使用时能够快速引用。

<!--more-->

## 块引用

在引用一些经典名言名句时，可以采用此短语，语法参考如下：

```markdown
{{</* quote */>}}
  #### block quote
  写下你想表达的话语！
{{</* /quote */>}}
```

实际效果：

{{< quote >}}

希望是无所谓有，无所谓无的，这正如地上的路。


其实地上本没有路，走的人多了，也便成了路。

**鲁迅**

{{< /quote >}}

## 信息块

支持 `default`，`info`，`success`，`warning`，`danger` 等五种不同效果的展示，语法参考如下：

```markdown
{{</* note [class] [no-icon] */>}}
  书写表达的信息
  支持 Markdown 语法
{{</* /note */>}}
```

实际效果：

{{< note default no-icon >}}
  #### Default Header without icon
  **Welcome** to [Hugo NexT!](https://preview.hugo-next.eu.org)
{{< /note >}}

{{< note default >}}
  #### Default Header
  **Welcome** to [Hugo NexT!](https://preview.hugo-next.eu.org)
{{< /note >}}

{{< note info >}}
  #### Info Header
  **Welcome** to [Hugo NexT!](https://preview.hugo-next.eu.org)
{{< /note >}}

{{< note success >}}
  #### Success Header
  **Welcome** to [Hugo NexT!](https://preview.hugo-next.eu.org)
{{< /note >}}

{{< note warning >}}
  #### Warning Header
  **Welcome** to [Hugo NexT!](https://preview.hugo-next.eu.org)
{{< /note >}}

{{< note danger >}}
  #### Danger Header
  **Welcome** to [Hugo NexT!](https://preview.hugo-next.eu.org)
{{< /note >}}
