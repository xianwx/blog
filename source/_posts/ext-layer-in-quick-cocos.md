---
title: 给lua文件注入插件的方法
date: 2017-01-07 11:54:39
description: "让代码变得更加简洁高效"
categories:
- 技术学习
tags: 
- lua
- quick cocos
---
## 1. 初衷
好久没有更新博客了，之前写了lua元表相关的东西，如果对元表不理解的话，可以在站内搜索一下**lua的元表以及多继承**。
现在在用quick cocos在写一款游戏，等空闲了可以来更新一下躺过的坑。
闲话少说，现在用cocos开发，走的路子多是策划或者美术拼出界面(cocos studio)，然后到处csb文件程序加载，再对界面进行各项处理(creator没用过)。
于是问题来了，一般的做法，将csb读取过来作为layer的child，可能会出现非常多次的对其getChild（可能层次很深），对csb上的某个控件进行setString、setTexture等，怎么做比较好？
## 2. 解决方案
提供几个解决方案：
```
* 写一个公用方法，获取csb的控件，每次都先取得控件后对其操作
* 写很多个公用方法，对setString的操作啊，对setTexture的操作啊都写上
* 写很多个local方法，代码一样
* 也是写很多个公用方法，分成各个插件，layer自己选择要去组装哪些
```
## 3. 插件的实现方法
非常简单，只需要几步
* 写插件，e.g:
``` lua
local ext = {}
function ext:setString()
    -- do set string
end

return function (layer)
    for k, v in pairs(ext) do
        -- 这里加上assert是怕覆盖了原有的字段
        assert(not layer[k], string.format("funciton %s exist", k))
        layer[k] = v
    end
end
```

* 注入
``` lua
local layer = display.newLayer()
require("ext")(layer)
```

* 使用
这样就可以直接使用
``` lua
layer:setString()
```

## 4. 总结
使用这种注入的方法，可以大幅度减少代码的重复量，也可以不用增加很多公用的方法，安全且高效。
注入的方法重点是ext的return 函数，对于**使用.**的方法需要有另外的写法。
等空闲时间，我会整理一个管理scene layer的框架，包括读取csb等框架传到github上。