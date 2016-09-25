---
title: 自定义自己的评论系统
date: 2016-09-16 18:18:13
description: 多说样式美化
categories:
- 技术学习
tags: 
- 多说
- hexo
---
今天逛本博客主题的编写者的[博客](http://moxfive.xyz/)的时候，看到了他的评论系统效果十分的好看，于是好奇心起，也打算自己弄一个一样的。
## 1. 已经做好的文件
在多说设置里用的[css文件](https://xianwx.github.io/show/test.css)
样式的[ejs文件](https://xianwx.github.io/show/embed.js)
本地修改的[ejs文件](https://xianwx.github.io/show/duoshuo.txt)
## 2. 把embed.js传到云端
把刚才下载的`样式的ejs文件`上传到一个任意的地方
```
比如我是复制到本地blog/themes/yelee/source/js/下边，上传到git后访问的地址就是https://xianwx.github.io/js/embed.js)
```
## 3. 浏览器、系统标识
* 使用上边的`本地修改的ejs文件`，把内容整个拷贝粘贴到`yelee/layout/_partial/comments/duoshuo.ejs`（注：使用跟我一样主题的人用）
* 其它主题的人：找到多说的ejs，给其控件的src赋值为你的embed.ejs的地址
``` js
var duoshuoQuery = {short_name:"<%=theme.duoshuo.domain%>"};
(function() {
    var ds = document.createElement('script');
    ds.type = 'text/javascript';ds.async = true;

    `ds.src = 'http://moxfive.xyz/resources/embed.js'`;
    /*上面是我自己的 embed.js 链接，请改为你自己的地址*/

    ds.charset = 'UTF-8';
    (document.getElementsByTagName('head')[0] 
     || document.getElementsByTagName('body')[0]).appendChild(ds);
})();
```

## 4. 引入 Font Awesome
打开theme/你的主题/layout/_partial/head.ejs
输入
``` html
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/font-awesome/4.4.0/css/font-awesome.min.css">
```
`如果已经有了就不需要加了`
## 5. 添加css
打开`http://你的多说域名.duoshuo.com/admin/settings/`
拷贝`样式的ejs文件`的内容
按照[后台添加css](http://wsgzao.github.io/post/duoshuo/#多说后台自定义CSS)里的步骤，添加自定义css
### ① 博主标志
在评论里，点自己的头像，进入http://duoshuo.com/settings/avatar/页面，点自己名字，进入http://duoshuo.com/profile/你的uid/界面，复制你的uid
打开刚才的embed.js文件，把你的uid复制到里边“你的uid”位置
``` js
function sskadmin(e) {
    var ssk = '';
    if (e.user_id == 你的uid) {
        if (checkMobile()) {
            ssk = '<span class="ua"><span class="sskadmin">博主</span></span><br><br>';
        } else {
            ssk = '<span class="ua"><span class="sskadmin">博主</span></span>';
        }
    } else {
        if (checkMobile()) {
            ssk = '<br><br>';
        }
    }
    return ssk;
}
```
并确保刚才添加到后台的css里有以下代码（刚才下的已经自带了）：
``` css
/*博主标记 CSS*/
.sskadmin {
    background-color: #00a67c!important;
    border-color: #01B171!important;
    border-radius: 4px;
    padding: 0 5px!important;
    opacity: .4;
}
.sskadmin:hover {
    opacity: 1;
}
```
**以下内容上文提供的下载的css都已经有了，只是讲解其功能，不喜欢可以删掉**
### ② 鼠标悬停旋转头像
``` scss
/*头像样式*/
#ds-reset .ds-avatar{background:none !important; box-shadow:none !important;}
#ds-reset .ds-avatar img , #ds-thread #ds-reset ul.ds-children .ds-avatar img{width:50px !important;height: 50px !important;-webkit-transition: .9s;-moz-transition: .9s;-o-transition: .9s;-ms-transition: .9s;padding: 2px;border: 1px solid #ddd;background: #fff;}
/*鼠标悬停旋转头像*/
.ds-post:hover .ds-avatar img{transform:rotate(360deg);-webkit-transform:rotate(360deg);-moz-transform:rotate(360deg);-o-transform:rotate(360deg);-ms-transform:rotate(360deg);border-radius:30px !important;}
#ds-reset .ds-avatar img:hover{transform:rotate(360deg);-webkit-transform:rotate(360deg);-moz-transform:rotate(360deg);-o-transform:rotate(360deg);-ms-transform:rotate(360deg);border-radius:30px !important;}
```
### ③ 通用样式
``` scss
#ds-thread #ds-reset .ds-comment-body, #ds-thread #ds-reset ul.ds-children .ds-comment-body{padding-left:70px !important;}
#ds-thread #ds-reset .ds-comment-body, #ds-thread #ds-reset ul.ds-children .ds-comment-body{padding-left:70px !important;}
.ds-post:hover{background:#eee !important;}
#ds-thread #ds-reset ul.ds-children .ds-avatar{width:50px !important;}
#ds-thread #ds-reset .ds-replybox{padding: 0 0 0 80px !important;}
#ds-reset #ds-ctx .ds-ctx-entry .ds-ctx-body{margin-left: 68px !important;}
#ds-recent-comments li.ds-comment:nth-of-type(1){border:none !important;}
#ds-thread{ 
    border-radius: 3px;
}
/** 多说最近留言样式 **/
#ds-recent-comments .ds-avatar img{   
    width:54px;height:54px; 
    border-radius: 50%; 
    -webkit-border-radius: 50%; 
    -moz-border-radius:50%;   
    box-shadow: inset 0 -1px 0 #3333sf;/*设置图像阴影效果*/  
    -webkit-box-shadow: inset 0 -1px 0 #3333sf;   
    -webkit-transition: 0.4s;      
    -webkit-transition: -webkit-transform 0.4s ease-out;   
    transition: transform 0.4s ease-out;
    -moz-transition: -moz-transform 0.4s ease-out;   
}    
#ds-recent-comments .ds-avatar img:hover{  
    box-shadow: 0 0 10px #fff; rgba(255,255,255,.6), inset 0 0 20px rgba(255,255,255,1);   
    -webkit-box-shadow: 0 0 10px #fff; rgba(255,255,255,.6), inset 0 0 20px rgba(255,255,255,1);   
    transform: rotateZ(360deg);
    -webkit-transform: rotateZ(360deg);   
    -moz-transform: rotateZ(360deg);   
}
```
### ④ 按钮样式
修改了透明度、背景、边框样式
``` css
.ds-meta {
    opacity: .5;
    }
.ds-meta:hover {
    opacity: 1;
    }
#ds-thread #ds-reset a.ds-like-thread-button {
    background-image: none;
    background-color: #fee2d3;
    border: none;
    text-shadow: none;
    font-family: inherit;
    }
```
### ⑤ 剩余自定义样式查看参考资料
> [多说样式折腾记录 — 添加 UA 浏览器标识、旋转头像等](http://moxfive.xyz/2015/09/29/duoshuo-style/)

## 参考资料
> [多说样式折腾记录 — 添加 UA 浏览器标识、旋转头像等](http://moxfive.xyz/2015/09/29/duoshuo-style/)
> [多说后台自定义css](https://wsgzao.github.io/post/duoshuo/#多说后台自定义CSS)