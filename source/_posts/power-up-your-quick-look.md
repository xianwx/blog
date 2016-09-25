---
title: 加强你的quick look
description: 让预览来的更多一些吧！
date: 2016-09-09 10:21:17
categories:
- 生活所得
tags: mac
---
## quick look
用mac的同学经常用`空格`键来预览一些文件的内容吧，在只想观看但是不打算修改内容的话用quick look正好，比如看文本，看psd的图片。
但是系统自带的只支持预览一些文本以及图像。

## 扩展
```
1. 安装brew
    自行Google
2. 安装brew cask
    brew tap phinze/homebrew-cask && brew install brew-cask
3. 使用命令行来安装插件：brew cask install ***
    
```
## 插件名收集
* 比如写博客要用到markdown，装个预览.md的插件吧：brew cask install qlmarkdown
* 需要预览无后缀名的文件：brew cask install qlstephen
* 程序员喜欢的，可以高亮代码的（可惜不能高亮lua：brew cask install qlcolorcode
* 需要预览json的（也可以到[kjson](http://www.kjson.com/jsoneditor/)：brew cask install quicklook-json
* 如果有安装BetterZip，安装这个可以通过BetterZip查看压缩文件的信息以及文件目录：brew cask install betterzipql
* 做游戏的可能会需要预览csv文件：brew cask install quicklook-csv
* 预览图片的时候显示分辨率和大小(过程中可能需要输入密码：brew cask install qlimagesize