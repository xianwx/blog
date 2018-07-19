---
title: 持续完善hexo博客
description: "持续性博客作死记录"
date: 2018-05-19 10:07:33
tags: hexo
categories:
- 技术学习
---
把博客搭建起来都一年多了，因为一直没啥人看，所以都懒得更新，现在想想，岂不是浪费了当初搭建的那半个多小时？

以后如果对博客有啥修改，就统一在这里更新。

update 2018-05-19:
## 一般博客都需要有的一个功能：置顶
修改`node_modules/hexo-generator-index/lib/generator.js`
加上
```js
posts.data = posts.data.sort(function(a, b) {
        if(a.top && b.top) { // 两篇文章top都有定义
            if(a.top == b.top) return b.date - a.date; // 若top值一样则按照文章日期降序排
            else return b.top - a.top; // 否则按照top值降序排
        }
        else if(a.top && !b.top) { // 以下是只有一篇文章top有定义，那么将有top的排在前面（这里用异或操作居然不行233）
            return -1;
        }
        else if(!a.top && b.top) {
            return 1;
        }
        else return b.date - a.date; // 都没定义按照文章日期降序排
    });
```
即可以用top值（默认0）来进行排序，数字越大的，就会排在越前面，top相等则以日期排序
参考[Hexo搭建博客教程](https://thief.one/2017/03/03/Hexo%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2%E6%95%99%E7%A8%8B/)

## 一般人都喜欢干的事情：添加音乐
好久之前就给自己博客加了音乐，但是！
我完全忘了是怎么加的了！
* 打开网易云网页版
* 登陆
* 打开自己的歌单，有个生成外链播放器的按钮
* 获取到外链代码，修改`layout/_partial/left-col.ejs`比如：
```js

<div id="music-area" class="switch-area">
    <iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=290 height=310 src="//music.163.com/outchain/player?type=0&id=523244238&auto=1&height=380&qlrc=1"></iframe>
</div>
```

## 给博客文章插入图片
可以把图片放在source目录下边，比如新建一个images文件夹，既可以用这种方式访问到它们。
```markdown
![](/images/14D6CC4556F6F4E73E2FB578D0D2F26E.jpg)
```
参考：[Hexo博客搭建之在文章中插入图片](https://yanyinhong.github.io/2017/05/02/How-to-insert-image-in-hexo-post/)

## 给博客文章引用站内其它文章
{% post_link 文章文件名（.md） 文章标题（可选） %}
e.g:
```markdown
{% post_link node-start-server Node开启一个HTTP服务器 %}
```