# Activity的四种启动模式及其应用场景
## 任务栈
程序打开时就创建了一个任务栈, 用于存储当前程序的activity,只有处于栈顶的activity才能与用户进行交互。当把所有activity清除时任务才会销毁。
## Activity启动模式
* STANDARD
 不管有没有已存在的实例，都生成新的实例。
* SINGLETOP
 如果发现有对应的Activity实例正位于栈顶，则重复利用，不再生成新的实例。否则会重新生成新的实例。
* SINGLETASK
 如果发现有对应的Activity实例，则使此Activity实例之上的其他Activity实例统统出栈，使此Activity实例成为栈顶对象，显示到幕前。
* SINGLEINSTANCE
启用一个新的栈结构，将Activity放置于这个新的栈结构中，并保证不再有其他Activity实例进入。

# Service的两种启动方式
## 使用startService启动服务的生命周期方法
* 使用这种方法启动一个Service，在Service启动后该Service和启动该Service的Activity就没有关系了。所以这种发放启动的Service不能和Activity进行交互。
* 通常情况下使用startService调用的Service生命周期方法为：onCreate()->onStartCommand()。其中多次调用startService只调用一次onCreate(), 但可以多次调用onStartCommand()。
* 当服务需要退出时，调用stopService，就会调用Service的onDestroy()方法。
## 使用bindService启动服务的生命周期方法
* 使用这种方法启动的Service是和调用者Activtiy同生命的，当Activtiy退出时，服务也同时销毁了。这种方法启动的Service能够和Activity进行交互。
* 调用bindService启动服务，Service生命周期方法为：onCreate()->onBind()多次调用bindService并不会多次调用onBind(),即onCreate()和onBind()都是只被调用一次。
* 当Activity退出时，该Service销毁，调用：onUnbind()->onDestroy();
# [Broadcast注册方式与区别](https://blog.csdn.net/carson_ho/article/details/52973504)
静态注册是常驻广播，不受任何组件生命周期的影响，只有在时刻监听广播的时候使用。缺点是耗电占内存。  动态注册则比较灵活可以控制其生命周期，在特定的情况下监听使用。
## 静态注册(minifest)
在AndroidManifest.xml里通过标签声明,当此App首次启动时，系统会自动实例化mBroadcastReceiver类，并注册到系统中。
## 动态注册
在代码中通过调用Context的registerReceiver（）方法进行动态注册BroadcastReceiver,动态广播最好在Activity的onResume()注册、onPause()注销。
