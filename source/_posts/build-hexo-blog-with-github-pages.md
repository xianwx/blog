---
title: 如何使用github + hexo搭建一个属于自己的博客
description: "自己的才是最好的！"
categories:
- 技术学习
date: 2016-09-08 19:36:27
tags: hexo
---
## 1. 简介
hexo是一个静态博客程序，基于Node.js。有很多主题，大多都简约优雅，适合技术人员以及一些不喜欢博客里充斥着广告，奇怪的布局的人。而且hexo用markdown写，语法比较简单。
github可以托管一个通过特殊的地址访问到的page，可以通过hexo创建好博客然后托管到github上，空间也基本足够。
我使用它是因为我写博客只是想记录一些东西，自己的记忆力太差。而且如果分享出来如果有人一起研究探讨，甚至能帮助到其他人，那是多么快乐的一件事情。
## 2. 配置
**本文是for Mac**
Mac自带了[git](https://git-scm.com/downloads)
配置好ssh
安装[Node.js](http://nodejs.org/)
确保npm安装成功
## 3. 在github上创建创库
每个github账户都可以创建一个可以直接通过‘用户名.github.io’访问到的仓库。
步骤：
```
① 在github上新建一个responsitory
② 给仓库命名为自己的用户名.github.io
③ 保存，过一小段时间就可以通过`用户名.github.io`访问了。
④ clone到本地，本文假设clone到一个名为blog的文件夹下。
```
## 4. 安装hexo
使用命令行安装：
```
npm install hexo-cli -g   #-g表示全局安装, npm默认为当前项目安装
```
## 5. 运行hexo
```
$ cd blog
$ hexo init #初始化
$ hexo g #这条是生成静态界面，也可以用hexo generate，g是缩写
$ hexo s #hexo server的缩写，启动服务器，现在你可以打开`http://localhost:4040/`看到你的界面了，初始化会创建好一个页面
```
## 6. 设置你想要的主题
我使用的主题是**[yelee](https://github.com/MOxFIVE/hexo-theme-yelee)**
很简单，运行
```
$ git clone https://github.com/MOxFIVE/hexo-theme-yelee.git themes/yelee
```
然后修改blog下的_config.yml文件
`theme: yelee`
## 7. 部署到github
修改_config.yml
```
deploy:
  type: git
  repository: https://github.com/xianwx/xianwx.github.io.git(这里改成你自己的刚才新建的github responsitory)
  branch: master 
```
安装插件
```
npm install hexo-deployer-git --save
```
设置成功过后执行
```
$ hexo d #deploy的缩写，首次会让你输入账号密码
```
等待上传成功，可以打开**[http://xianwx.github.io](http://xianwx.github.io)**看效果啦！

-- todo 新建一个分支保存博客程序，避免换电脑的时候悲剧
## 8. 添加多说
```
① 到http://duoshuo.com/create-site/新建一个站点
② 站点地址填http://xianwx.github.io 这个，域名随便写个，比如我是yuzixin

如果你使用跟我不一样的主题，跳过不看后面这步，去自己看主题里的配置方法。
③ 打开yelee/_config.yml，找到duoshuo，把on的值改为true，domain: 写自己的域名（上边的yuzixin）
```
## 9. 接入打赏
打开themes/yelee/layout/_partial/article.ejs下边，在div模块加上下边代码
[代码段](https://xianwx.github.io/show/article.txt)

然后在主题的_config.yml里加上
```
donate:
  enable: true
  text: 会不会有人就是想不开要打赏我呢？这是个问题！
  wechat: http://odkw6ym6r.bkt.clouddn.com/weixin.png
  alipay: http://odkw6ym6r.bkt.clouddn.com/zhifubao.png
```
## 10. 插件配置
在_config.yml里，有个plugins
在安装了插件过后，就可以在这里选择使用格式是:
```
Plugins: -hexo-generator-feed -hexo-generator-search
```
(我分行写会报错，只能一整行写过去)
更多plugins：[https://hexo.io/plugins/](https://hexo.io/plugins/)
## 11. 图床
到[https://portal.qiniu.com/create](https://portal.qiniu.com/create)注册一个账号
参考[如何使用七牛](http://www.jianshu.com/p/6dce6094bf61)
## 12. 如何保存自己的博客
今天想了一下怎么保存，思路是：
    相同的仓库下新建一个目录，或者新建一个仓库
    clone下来跟blog关联（还没开始的先clone下来然后hexo init那个文件夹，已经init过了clone下来然后把东西拷贝过去）
    如果跟博客是相同的目录，checkout到保存的分支，反正使用hexo d是会推送到master分支的，也指定好仓库的，所以不需要担心
    每次有修改push一下就好了。
## 13. about me and tabs
想写about me的话，使用hexo new about me.
## 14. 剩下的一些杂七杂八的配置
预告：rss，写博客的方法，写about me，markdown语法，-- todo 自己还需要研究的：购买域名
先欠着，往后补上，回家去咯~