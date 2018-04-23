# Android基础面试题
## Android系统的架构
1. 应用层(System App)  
    系统内置应用与非内置应用，Java开发直接与用户交互。
2. 应用框架层(Java API Framework)  
    为开发人员提供开发API，主要包含以下组件  
    1. ActivityManager  
    2. LocationManager  
    3. PackageManager  
    4. NotificationManager  
    5. ResourceManager  
    6. TelephonyManager  
    7. WindowManager  
    8. ContentManager  
    9. ViewSystem  
3. 系统运行库层(Native)  
    c/c++开发主要分为连个部分
    1. c/c++程序库
    2. Android运行时库
4. 硬件抽象层(HAL)
5. Linux内核
##  Android中4大组件
1. Activity  
    展示型组件，由Intent启动(包括点击图标启动的入口Activity)，运行模式为启动状态。
2. Service  
    计算型组件，有startService或者bindService启动，运动模式有启动和绑定两种。
3. BroadcastReceiver  
    消息类组件，通过注册方式启动，分为静态注册和动态注册。
4. ContentProvider  
    数据共享型组件。
## [Activity的生命周期](https://blog.csdn.net/android_tutor/article/details/5772285)
## [Fragment的生命周期](https://blog.csdn.net/lmj623565791/article/details/37970961)
## [Service生命周期](https://blog.csdn.net/guolin_blog/article/details/11952435)
## [Broadcast注册方式与区别](https://blog.csdn.net/carson_ho/article/details/52973504)
静态注册是常驻广播，不受任何组件生命周期的影响，只有在时刻监听广播的时候使用。缺点是耗电占内存。  动态注册则比较灵活可以控制其生命周期，在特定的情况下监听使用。
### 静态注册(minifest)
在AndroidManifest.xml里通过标签声明,当此App首次启动时，系统会自动实例化mBroadcastReceiver类，并注册到系统中。
### 动态注册
在代码中通过调用Context的registerReceiver（）方法进行动态注册BroadcastReceiver,动态广播最好在Activity的onResume()注册、onPause()注销。
## Android中动画
[1. Frame Animation](https://blog.csdn.net/liuhe688/article/details/6657776)  
[2. Tween Animation](https://blog.csdn.net/liuhe688/article/details/6660823)  
[3. Layout Animation](https://blog.csdn.net/guolin_blog/article/details/43536355)  
[4. Property Animation](https://blog.csdn.net/guolin_blog/article/details/43536355)
## Android中常用布局
## Android的数据存储
## Android中的ANR
## [ListView详细讲解](https://blog.csdn.net/guolin_blog/article/details/44996879)
## View、surfaceView、GLSurfaceView
## AsyncTask

## Activity的四种启动模式及其应用场景
### 任务栈
程序打开时就创建了一个任务栈, 用于存储当前程序的activity,只有处于栈顶的activity才能与用户进行交互。当把所有activity清除时任务才会销毁。
### Activity启动模式
* STANDARD
 不管有没有已存在的实例，都生成新的实例。
* SINGLETOP
 如果发现有对应的Activity实例正位于栈顶，则重复利用，不再生成新的实例。否则会重新生成新的实例。
* SINGLETASK
 如果发现有对应的Activity实例，则使此Activity实例之上的其他Activity实例统统出栈，使此Activity实例成为栈顶对象，显示到幕前。
* SINGLEINSTANCE
启用一个新的栈结构，将Activity放置于这个新的栈结构中，并保证不再有其他Activity实例进入。

## Service的两种启动方式
### 使用startService启动服务的生命周期方法
* 使用这种方法启动一个Service，在Service启动后该Service和启动该Service的Activity就没有关系了。所以这种发放启动的Service不能和Activity进行交互。
* 通常情况下使用startService调用的Service生命周期方法为：onCreate()->onStartCommand()。其中多次调用startService只调用一次onCreate(), 但可以多次调用onStartCommand()。
* 当服务需要退出时，调用stopService，就会调用Service的onDestroy()方法。
### 使用bindService启动服务的生命周期方法
* 使用这种方法启动的Service是和调用者Activtiy同生命的，当Activtiy退出时，服务也同时销毁了。这种方法启动的Service能够和Activity进行交互。
* 调用bindService启动服务，Service生命周期方法为：onCreate()->onBind()多次调用bindService并不会多次调用onBind(),即onCreate()和onBind()都是只被调用一次。
* 当Activity退出时，该Service销毁，调用：onUnbind()->onDestroy();

## Android消息机制
Android的消息机制一般指的是Handler的运行机制。Handler的运行机制离不开MessageQueue/Message/Looper/Handler这四个类。
Message：消息产生分为硬件生成和软件生成。
MessageQueue：主要功能是向消息池投递消息(MessageQueue.enqueueMessage)和取走消息(MessageQueue.next)。
Handler：消息辅助类，主要功能向消息池发送各种消息事件(Handler.sendMessage)和处理相应消息事件(Handler.handleMessage)。
Looper：不断循环执行(Looper.loop)，按分发机制将消息分发给目标处理者。
## Android 线程池的实现原理
## Android屏幕渲染机制
