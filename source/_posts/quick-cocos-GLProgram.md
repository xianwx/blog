---
title: 把quick-cocos-community 3.6.1里的GLProgram里隐藏的接口暴露给Lua
date: 2018-08-22 14:57:13
description: 拒绝阉割，从我做起。
categories: 技术学习
tags:
- quick
- cocos
- shader
---
不知道为什么3.6.1里把GLProgram的接口给屏蔽了，导致想用shader的时候会有很多的不方便，于是决定把接口再次暴露出来。
# 修改导出配置的ini文件
open `quick/tools/tolua/cocos2dx.ini`，cocos系统的C++类大部分都在里边导出的，自定义的类可以新增ini文件用于导出，并新增对应的genbindings python脚本，新增的好处是，不需要把系统的重新导出一遍，一是节省时间，二是。。。。用genbindings.py文件重新导出的话，会有大量的diff，`这一点朕也不明白`。

言归正传，打开`cocos2dx.ini`后，可以看到在skip里skip掉了GLProgram里的大量接口（用的是正则式，干掉了一堆），直接都删掉，执行genbindings.py脚本重新导出。

# 解决报错
按理来说！上述过程做完就算成功了，但是！cocos是不可能让你如愿的（计划通），首先，导出过后，所有的lua文件里的return self几乎都会被删掉，导出的cpp文件里的
```cpp
  lua_settop(tolua_S, 1);
  return 1;
```
也大量会变成
```cpp
  return 0
```
很无奈吧。。。。我的做法就是，手动剔除掉跟此次修改无关的地方的diff，只把新增的部分copy过来，具体怎么操作，就只是个体力活而已了，没啥好说的。
但是这还没完，如果你用xcode重新编译，就会有以下报错，大意是有7处重复了。
```
ld: 7 duplicate symbols for architecture x86_64
```

是不是莫名其妙？
这时候我们才突然想起来，没错，里边还有个文件叫：`lua_cocos2dx_manual.cpp`
惊喜不惊喜？意外不意外？没错，这里手写了一部分接口，所以导致了重复，所以只要把`lua_cocos2dx_auto.cpp`里的已经manual导出过的接口给删除掉即可。

# 需要注意的地方
查看GLProgram.lua文件，仔细看注释，`setUniformLocationWith1f`，`setUniformLocationWith2f`，`setUniformLocationWith3f`，`setUniformLocationWith4f`，`setUniformLocationWith1i`这几个方法，分别被转成了`setUniformLocationF32`，`setUniformLocationI32`两个方法。不要用错咯。

# bugs
使用`setUniformFloat`方法时会有`cc.GLProgramState:setUniformFloat argument #2 is 'string'; 'number' expected.`的报错，明明参数是没传错的，于是查看`lua_cocos2dx_auto.cpp`文件里`setUniformFloat`的实现，发现有串重复的代码，删掉即可。就是这串：
```cpp
    do{
        if (argc == 2) {
            int arg0;
            ok &= luaval_to_int32(tolua_S, 2,(int *)&arg0, "cc.GLProgramState:setUniformFloat");

            if (!ok) { break; }
            double arg1;
            ok &= luaval_to_number(tolua_S, 3,&arg1, "cc.GLProgramState:setUniformFloat");

            if (!ok) { break; }
            cobj->setUniformFloat(arg0, arg1);
            lua_settop(tolua_S, 1);
            return 1;
        }
    }while(0);
    ok  = true;
```

同理，`setUniformVec2`也有同样的报错，用同样的解决方法即可。
# 结束
重新使用xcode编译出play，可以愉快的用shader了。之后我会分享一些特效在github和coding上，欢迎关注。