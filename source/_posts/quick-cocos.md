---
title: 写cocos的时候一些踩雷记录
description: "平常吐槽不停，真到写起来的时候反而记不起来了……"
date: 2018-07-19 11:48:05
tags: git
categories:
- 技术学习
---
# C++导出给LUA使用的时候
在导出给lua使用的时候，如果父类没有导出，那父控件继承的公共方法将没有。
比如 EffectLight 继承自 Effect Effect 继承自 Ref 如果不导出Effect 创建的EffectLight对象将没有retain方法，除非导出Effect。