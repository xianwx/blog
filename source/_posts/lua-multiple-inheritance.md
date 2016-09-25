---
title: lua的元表以及多继承
date: 2016-09-23 11:49:39
description: lua的元表以及多重继承的方法
categories: 技术学习
tags: lua
---
最近做开发的时候，有个类想要继承自多个类，虽然这种情况基本说很少（好像也不少），但还是说一下如何在lua里实现多继承。
其实很简单，首先要了解lua元表，以及lua类的实现方法。
## lua元表
用lua的人都知道lua的table的强大，table有一套hashmap的查找机制，如果访问一个表中并不存在的字段，不会立即返回nil，而是先`触发一套查找机制，也是我们用以实现面向对象的方法`。
简单的描述：`元表就是用于查找的备用表。`
比如：
``` lua
local me = {}
print(me.money) -- me中没有money字段，所以打印出来的会是nil
```
这里的执行结果很明显会是nil，没有疑问（因为我是个穷逼。
但是如果我设置了元表：
``` lua
local fathermayun = {
    money = 13000000000
}
local me = {}
setmetatable(me, fathermayun)
print(me.money)
```
这里的执行结果是！！！！！！！
`nil`
为什么我没有拿到马云爸爸的钱？因为没有设置`__index`
## __index
简单的描述：`是当table中一个元素不存在的时候，会触发寻找元表的__index元方法，如果不存在，则返回nil，如果存在，则返回结果。`
所以，把上述代码改成
``` lua
local fathermayun = {
    money = 13000000000
}
fathermayun.__index = fathermayun
local me = {}
setmetatable(me, fathermayun)
print(me.money)
```
这里的执行结果将是13000000000，我终于有钱了:)
上面的执行过程是：
访问my.money->发现没有这个字段->my有元表->查找元表fathermayun->lua不会直接找fathermayun里的money字段->lua发现fathermayun有元方法__index->调用元方法->发现元方法是个table，在元方法的table里寻找->获得money的值
__index有以下取值
```
表，会直接在表里找
函数，返回函数的返回值
```
## 继承
利用上述知识随手写了个实现面向对象的方法，不是很完善
``` lua
local function class(super)
    local cls
    if super then
        cls = {}
        cls.super = super
        setmetatable(cls, {__index = super})
    else
        -- ctor是构造函数的命名
        cls = {ctor = function () end}
    end

    cls.__index = cls
    function cls.new(...)
        local instance = setmetatable({}, cls)
        cls:ctor(...)
        return instance
    end
    return cls
end
```
挺简单的，注释就不写了……让我们来测试一下
```
local Test = class()
function Test:doSomething()
    print("test doSomething")
end
local test = Test.new()
test:doSomething()
```
成功打印出"test doSomething"
接下来试试继承
``` lua
local Test = class()
function Test:doSomething()
    print("test doSomething")
end
local Test2 = class(Test)
local test = Test2.new()
test:doSomething()
```
也可以成功打印出"test doSomething"，继承成功
为什么能继承成功？原因如下：
```
在new的时候，创建一个table并返回，即创建一个实例，实例可以有自己的字段，比如Test类的doSomething，该字段是个函数，可以调用执行。实例的元表是cls，如果调用实例没有的字段，会去cls里找
cls设置了元方法__index = cls
如果没有super，则只有一个构造函数方法
如果有super，cls的元表是super，元表的元方法也正确的设置了
所以，在Test2是继承自Test的，它的实例test调用doSomething，找不到，去元表里找，元表发现自己有父类，去父类里找，成功找到。
```
## 多继承
如果我想要继承多个父类，怎么办？
其实思路很简单，把`元方法改成函数即可。`
举例子：
``` lua
local function search(key, tables)
    for _, super in ipairs(tables) do
        if super[key] then
            return super[key]
        end
    end
    return nil
end

local function class(...)
    local cls = { ctor = function () end}
    local supers = {...}
    setmetatable(cls, {__index = function (_, key)
        -- 在查找table的时候，会把table的key传进来
        return search(key, supers)
    end})

    if next(supers) then
        -- print_r(supers)
        print_r(cls)
    end
    
    function cls.new(...)
        local instance = {}
        setmetatable(instance, {__index = cls})
        cls:ctor(...)
        return instance
    end
    return cls
end

local Human = class()
function Human:life()
    print("almost 100 years.")
end
local Programmer = class()
function Programmer:coding()
    print("sub 1 year.")
end
local My = class(Human, Programmer)
local yuzixin = My.new()
yuzixin:life()
yuzixin:coding()
```
成功打印出结果
almost 100 years.
sub 1 year.
为什么能继承成功？原因如下：

```
在yuzixin里找不到life和coding字段，去找元表cls，调用元方法__index
__index调用函数search，把所有的父类都找一遍
成功找到
```
多继承就这样成功啦！撒花！
注意写一次代码减少一年的生命哦……（悲伤doge脸