---
title: iterm2-zhs安装教程
date: 2021/5/11 20:00
tags: iterm2-zsh
categories: tools
---

# 安装 iterm2

官网：https://iterm2.com/downloads.html

## 安装 oh-my-zsh

> 官网：https://ohmyz.sh/#install

> github：https://github.com/ohmyzsh/ohmyzsh

| Method |                                             Command                                              |
| :----: | :----------------------------------------------------------------------------------------------: |
|  curl  | sh -c "\$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" |
|  wget  |  sh -c "\$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"  |
| fetch  | sh -c "\$(fetch -o - https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" |

# 主题配置

以 agnoster 主题为例：

ZSH_THEME 默认为 "robbyrussell"。

编辑.zshrc 文件将 ZSH_THEME 修改为 "agnoster"。

> vim cat ~/.zshrc

> ![编辑](https://note.youdao.com/yws/public/resource/0015110e4e9c35f172f81ce06b323df6/xmlnote/WEBRESOURCE9334cfc6f3cc850889d56e9945dc8ce7/6830)

此时打开 iterm2 会显示乱码，需要安装 powerline 字体和配置

## 安装 poweline 字体

github：https://github.com/powerline/fonts

### 推荐使用

```js
# clone
git clone https://github.com/powerline/fonts.git --depth=1

# install
cd fonts
./install.sh

# clean-up a bit
cd ..
rm -rf fonts
```

### iterm2 配置

打开 iterm2 设置面板

> Preferences -> Profiles -> Text

勾选 Use built-in Powerline glyphs 选项

![配置](https://note.youdao.com/yws/public/resource/0015110e4e9c35f172f81ce06b323df6/xmlnote/WEBRESOURCEa07e03d30df406a1e595319797dc3c5f/6834)

退出 iterm2 之后重新打开即可正确显示字体文字
