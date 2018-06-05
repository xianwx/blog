---
title: LuaJavaBridge
description: "让Lua和Java互操"
date: 2018-06-05 10:06:04
tags: cocos
categories:
- 技术学习
---
用quick cocos开发游戏的时候，如果你有接入过SDK，或者做过复制到剪贴板，获取设备信息等，肯定就会碰到Lua调用Java层的需求。
于是就需要用到：LuaJavaBridge。

在使用require "framework"过后，可以直接使用luaj（具体可以查看luaj.lua）
这篇文章主要记录如何使用。
## 1. 使用
luaj和luaoc都是调用的static方法
所以，在Java层加上一个public的static方法，如：
```java
public static void testLuaj(){
    Log.i("Luaj", "testLuaj");
}
```
quick cocos运行的主activity是AppActivity，完整类名称为：org/cocos2dx/lua/AppActivity
所以，在lua层可以使用如下代码调用：
```lua
luaj.callStaticMethod("org/cocos2dx/lua/AppActivity","testLuaj",{},"()V")
```

## 2. 参数介绍
第一个参数是完整的类名称，“.”需要使用“/”，如"org/cocos2dx/lua/AppActivity"
第二个参数是方法名，如"testLuaj"
第三个参数是参数，无参数的时候使用空table
第四个参数指定参数的类型和返回类型的签名

## 3. 参数类型
签名                                         解释
()V                             参数：无，返回值：无
(I)V                            参数：int，返回值：无
(Ljava/lang/String;)Z           参数：字符串，返回值：布尔值
(IF)Ljava/lang/String;          参数：整数、浮点数，返回值：字符串

类型名                 类型
I                       整数，或者 Lua function
F                       浮点数
Z                       布尔值
Ljava/lang/String;      字符串
V                       Void 空，仅用于指定一个 Java 方法不返回任何值

## 4. error code
错误代码                            描述
-1                          不支持的参数类型或返回值类型
-2                          无效的签名
-3                          没有找到指定的方法
-4                          Java 方法执行时抛出了异常
-5                          Java 虚拟机出错
-6                          Java 虚拟机出错

* 附：参数类型和error code懒得自己写，从[Vincent__Lee的CSDN博客](https://blog.csdn.net/li15225271052/article/details/70148609)拷贝过来的。

## 5. 返回值
luaj有两个返回值，一个是调用结果，一个是返回值。
如：
```java
public static String getProvider(){
    return "TapTap";
}
```
```lua
-- Ljava/lang/String;的;是不可省略的
local ok, result = luaj.callStaticMethod("org/cocos2dx/lua/AppActivity", "getProvider", {}, "()Ljava/lang/String;")
print("执行结果：", ok)
print("provider: ", result)
```

## 6. 特别介绍
可以将lua的方法作为int类型参数传入Java方法
也可以把table转换为json传入
```java
    public static void testLuaCallback(final int callback, final String args){
        AppActivity.activity.runOnUiThread(new Runnable() {
            @Override
            public void run() {
                try {
                    JSONObject argsJ = new JSONObject(args);

                    // 可以通过这种方法获取lua传入的值
                    String testId = argsJ.getInt("id")
                    String testVal = argsJ.getString("val");

                    // ok为传入到lua回调的参数
                    Cocos2dxLuaJavaBridge.callLuaFunctionWithString(callback, "ok");
                    Cocos2dxLuaJavaBridge.releaseLuaFunction(callback);

                } catch (JSONException e) {
                    e.printStackTrace();
                }
            }
        });
    }

```
```lua
    local function callback(result)
        print("result") -- 会打印出ok
    end
    
    local args = json.encode({ id = 1, val = "test" })
    luaj.callStaticMethod("org/cocos2dx/lua/AppActivity", "testLuaCallback", {callback, args}, "(ILjava/lang/String;)V")
```

## 7. 注意
直接使用LuaObjcBridge的时候要注意，参数必须是3个
LuaObjcBridge.callStaticMethod("AppController", "getProvider")
会导致执行失败
LuaObjcBridge.callStaticMethod("AppController", "getProvider", nil)
luaoc.callStaticMethod就是3个参数，少传一个是自动帮你传了nil过去，所以不会出错。