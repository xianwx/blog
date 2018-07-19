---
title: Android设置应用启动页
description: "Logo还是要的嘛，虽然安卓喜欢不要脸的……"
date: 2018-07-19 11:34:12
tags:
- Android
- quick cocos
categories:
- 技术学习
---
最简单的方法无非是在游戏里设置……这样苹果和安卓都有了，还好加各种特效。
重点说一下给安卓加启动图，思路是：加一个新的activity，sleep一会儿，再跳到主activity。
好，新建一个activity叫SplashActivity，继承自Activity，不要继承自cocos的activity，它会自动去帮你走AppDelegate……
参考以下代码：
```java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        getWindow().addFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN);
        // 常亮
        getWindow().addFlags(WindowManager.LayoutParams.FLAG_KEEP_SCREEN_ON);

        // 横屏旋转
        setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_SENSOR_LANDSCAPE);

        //隐藏虚拟按键，并且全屏
        final Window window = getWindow();
        setHideVirtualKey(window);
        window.getDecorView().setOnSystemUiVisibilityChangeListener(new View.OnSystemUiVisibilityChangeListener() {
            @Override
            public void onSystemUiVisibilityChange(int visibility) {
                setHideVirtualKey(window);
            }
        });

        Log.i("LUA", "on splash activity create");
               /*mainLayout初始化*/

        LinearLayout mainLayout = new LinearLayout(this);
        mainLayout.setBackgroundColor(Color.WHITE);
        mainLayout.setLayoutParams(new LinearLayout.LayoutParams(-1,-1));
        mainLayout.setGravity(17); // 17 的意义是 "CENTER"
        /*iv初始化*/
        ImageView iv = new ImageView(this);
        iv.setLayoutParams(new LinearLayout.LayoutParams(-1,-2));
        iv.setScaleType(ImageView.ScaleType.CENTER);//居中显示
        int resId=this.getResources().getIdentifier("bg","drawable",getPackageName());
        iv.setImageResource(resId);
        mainLayout.addView(iv);//添加iv
        setContentView(mainLayout);//显示manLayout

        Thread myThread=new Thread(){//创建子线程
            @Override
            public void run() {
                try{
                    sleep(2000);//使程序休眠五秒
                    Log.i("LUA", "splash sleep end");
                    Intent it=new Intent(getApplicationContext(), AppActivity.class);//启动MainActivity
                    startActivity(it);
                    finish();//关闭当前活动
                }catch (Exception e){
                    e.printStackTrace();
                }
            }
        };

        myThread.start();//启动线程
    }

    public void setHideVirtualKey(Window window){
        //保持布局状态
        int uiOptions = View.SYSTEM_UI_FLAG_LAYOUT_STABLE|
                //布局位于状态栏下方
                View.SYSTEM_UI_FLAG_LAYOUT_HIDE_NAVIGATION|
                //全屏
                View.SYSTEM_UI_FLAG_FULLSCREEN|
                //隐藏导航栏
                View.SYSTEM_UI_FLAG_HIDE_NAVIGATION|
                View.SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN;
        if (Build.VERSION.SDK_INT>=19){
            uiOptions |= 0x00001000;
        }else{
            uiOptions |= View.SYSTEM_UI_FLAG_LOW_PROFILE;
        }
        window.getDecorView().setSystemUiVisibility(uiOptions);
    }

    @Override
    protected void onResume() {
        super.onResume();
        final Window window = getWindow();
        setHideVirtualKey(window);
    }
```
大概做的事情就是，创建一个layout，然后在layout里创建一个ImageView，图片为bg，为了使ImageView居中，所以设置了父控件的gravity，显示完了，创建一个线程，sleep2秒钟，跳到AppActivity，splash这个activity横屏且隐藏状态栏和虚拟按钮。
最后，记得把**AndroidManifest**里的启动activity换成新建的这个activity。

另外，为了让苹果的启动图时间久一点，可以在didFinishLaunchingWithOptions里加上一句
```object-c
    // 启动图片延时: 1秒
    [NSThread sleepForTimeInterval:1];

```
经实测，1秒的效果蛮好的。