---
title: iTerm2 使用指南
author: J Song
date: '2024-09-06'
slug: []
categories:
  - 技术文章
  - 编程
description: 2024-09-06-terminal-learning-notes-2
keywords: 2024,09,06,terminal,learning,notes,2
lastmod: '2024-09-06T11:45:09+08:00'
---
> 感谢 **村雨 Mura** 的[帖子](https://www.bilibili.com/read/cv15324398/)

## 1. 安装 [iTerm2](https://iterm2.com) 

改变主题：https://iterm2colorschemes.com

## 2. 安装 zsh
苹果电脑默认是 zsh，可以跳过这一步

## 3. 安装 [ohmyzsh](https://github.com/ohmyzsh/ohmyzsh)
- 把 code 下载到 home 路径下，再解压（如果提示说已经有.oh-my-zsh 文件夹，可能之前安装过，可以删掉文件夹再安装）

- 解压后进入解压目录
```bash
cd ohmyzsh-master
sh ./tools/install.sh
## sh ./tools/uninstall.sh ## 卸载
```
- 退出重启

## 4. 更改 zsh 主题
- 主题预览网址：https://github.com/ohmyzsh/ohmyzsh/wiki/Themes
- 修改 `~/.zshrc` 文件
```bash
vim ~/.zshrc
## 在打开的文件中修改或新增：
ZSH_THEME="steeef" ## 以 steeef 为例
## 退出文件
Esc + :wq
```
- 更新一下 `source ~/.zshrc`


## 5. 语法高亮
- 克隆下面代码到 `~/.oh-my-zsh/custom/plugins`
```
git clone git://github.com/zsh-users/zsh-syntax-highlighting $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
```

- 打开 `~/.zshrc`，找到 `plugins`，追加 `zsh-syntax-highlighting`
```
plugins=(git zsh-syntax-highlighting)
```

- 更新一下 `source ~/.zshrc`

## 6. 自动补全插件
- 克隆下面代码到 `~/.oh-my-zsh/custom/plugins`
```
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

- 打开 `~/.zshrc`，找到 `plugins`，追加 `zsh-autosuggestions`
```
plugins=(git zsh-syntax-highlighting zsh-autosuggestions)
```

- 更新一下 `source ~/.zshrc`

- 可以补全历史输入的命令，点击方向键 `->` 即可补全