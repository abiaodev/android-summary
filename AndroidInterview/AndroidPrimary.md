# Android基础面试题
## 1 Android系统的架构
### 1.1 应用层(System App)  
系统内置应用与非内置应用，Java开发直接与用户交互。
### 1.2 应用框架层(Java API Framework)  
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

### 1.3 系统运行库层(Native)  
c/c++开发主要分为连个部分
1. c/c++程序库
2. Android运行时库

### 1.4 硬件抽象层(HAL)
### 1.5 Linux内核
***
## 2  Android中四大组件
1. Activity  
      Activity 表示具有用户界面的单一屏幕。例如，电子邮件应用可能具有一个显示新电子邮件列表的 Activity、一个用于撰写电子邮件的 Activity 以及一个用于阅读电子邮件的 Activity。 尽管这些 Activity 通过协作在电子邮件应用中形成了一种紧密结合的用户体验，但每一个 Activity 都独立于其他 Activity 而存在。因此，其他应用可以启动其中任何一个 Activity（如果电子邮件应用允许）。例如，相机应用可以启动电子邮件应用内用于撰写新电子邮件的 Activity，以便用户共享图片。
2. Service  
      服务是一种在后台运行的组件，用于执行长时间运行的操作或为远程进程执行作业。 服务不提供用户界面。 例如，当用户位于其他应用中时，服务可能在后台播放音乐或者通过网络获取数据，但不会阻断用户与 Activity 的交互。诸如 Activity 等其他组件可以启动服务，让其运行或与其绑定以便与其进行交互。
3. BroadcastReceiver  
      广播接收器是一种用于响应系统范围广播通知的组件。 许多广播都是由系统发起的 — 例如，通知屏幕已关闭、电池电量不足或已拍摄照片的广播。应用也可以发起广播 — 例如，通知其他应用某些数据已下载至设备，并且可供其使用。   
    尽管广播接收器不会显示用户界面，但它们可以创建状态栏通知，在发生广播事件时提醒用户。 但广播接收器更常见  
    的用途只是作为通向其他组件的“通道”，设计用于执行极少量的工作。 例如，它可能会基于事件发起一项服务来执行某项工作。
4. ContentProvider  
    内容提供程序管理一组共享的应用数据。您可以将数据存储在文件系统、SQLite 数据库、网络上或您的应用可以访  
    问的任何其他永久性存储位置。 其他应用可以通过内容提供程序查询数据，甚至修改数据（如果内容提供程序允许）。   
    例如，Android 系统可提供管理用户联系人信息的内容提供程序。 因此，任何具有适当权限的应用都可以查询内容提  
    供程序的某一部分（如 ContactsContract.Data），以读取和写入有关特定人员的信息。
  内容提供程序也适用于读取和写入您的应用不共享的私有数据。 例如，记事本示例应用使用内容提供程序来保存笔记。

### 2.1 Activity的[生命周期](https://blog.csdn.net/android_tutor/article/details/5772285)以及[启动过程](http://gityuan.com/2016/03/12/start-activity/)
### 2.2 Activity的四种启动模式及其应用场景
#### 2.2.1 任务栈
  程序打开时就创建了一个任务栈, 用于存储当前程序的activity,只有处于栈顶的activity才能与用户进行交互。当把所有activity清除时任务才会销毁。
#### 2.2.2 Activity启动模式
* STANDARD
     不管有没有已存在的实例，都生成新的实例。
* SINGLETOP
     如果发现有对应的Activity实例正位于栈顶，则重复利用，不再生成新的实例。否则会重新生成新的实例。
* SINGLETASK
     如果发现有对应的Activity实例，则使此Activity实例之上的其他Activity实例统统出栈，使此Activity实例成为栈顶对象，显示到幕前。
* SINGLEINSTANCE
    启用一个新的栈结构，将Activity放置于这个新的栈结构中，并保证不再有其他Activity实例进入。

### 2.3 保存Activity状态

### 2.4 [Fragment的生命周期](https://blog.csdn.net/lmj623565791/article/details/37970961)

### 2.5 Service的[生命周期](https://blog.csdn.net/guolin_blog/article/details/11952435)以及[启动过程](http://gityuan.com/2016/03/06/start-service/)
### 2.6 Service的两种启动方式
#### 2.6.1 使用startService启动服务的生命周期方法
1. 使用这种方法启动一个Service，在Service启动后该Service和启动该Service的Activity就没有关系了。所以这种发放启动的Service不能和Activity进行交互。
2. 通常情况下使用startService调用的Service生命周期方法为：onCreate()->onStartCommand()。其中多次调用startService只调用一次onCreate(), 但可以多次调用onStartCommand()。
3. 当服务需要退出时，调用stopService，就会调用Service的onDestroy()方法。

#### 2.6.2 使用bindService启动服务的生命周期方法
1. 使用这种方法启动的Service是和调用者Activtiy同生命的，当Activtiy退出时，服务也同时销毁了。这种方法启动的Service能够和Activity进行交互。
2. 调用bindService启动服务，Service生命周期方法为：onCreate()->onBind()多次调用bindService并不会多次调用onBind(),即onCreate()和onBind()都是只被调用一次。
3. 当Activity退出时，该Service销毁，调用：onUnbind()->onDestroy();

### 2.7 [Broadcast注册方式与区别](https://blog.csdn.net/carson_ho/article/details/52973504)
静态注册是常驻广播，不受任何组件生命周期的影响，只有在时刻监听广播的时候使用。缺点是耗电占内存。  动态注册则比较灵活可以控制其生命周期，在特定的情况下监听使用。
#### 2.7.1 静态注册(minifest)
在AndroidManifest.xml里通过标签声明,当此App首次启动时，系统会自动实例化mBroadcastReceiver类，并注册到系统中。
#### 2.7.2 动态注册
在代码中通过调用Context的registerReceiver（）方法进行动态注册BroadcastReceiver,动态广播最好在Activity的onResume()注册、onPause()注销。


***
## 3 UI相关(使用)
### 3.1 Android中常用布局
#### 3.1.1 LinearLayout
#### 3.1.2 TableLayout
#### 3.1.3 RelativeLayout
#### 3.1.4 FrameLayout
#### 3.1.5 AbsoluteLayout
#### 3.1.6 GridLayout
#### 3.1.7 CoordinatorLayout
### 3.2 [WebView](https://www.jianshu.com/p/fd61e8f4049e)常用方法
  *   onPause() 尽力尝试暂停可以暂停的任何处理，如动画和地理位置。 不会暂停JavaScript。 要全局暂停JavaScript，可使用pauseTimers。
  *   onResume() 恢复onPause() 停掉的操作；
  *   pauseTimers() 暂停所有WebView的布局，解析和JavaScript定时器。 这个是一个全局请求，不仅限于这个WebView。
  *   resumeTimers() 恢复所有WebView的所有布局，解析和JavaScript计时器，将恢复调度所有计时器。
***
### 3.3 [WebView与js交互](https://blog.csdn.net/carson_ho/article/details/64904691/)
#### 3.3.1 对于Android调用JS代码的方法有2种
  1. 通过WebView的loadUrl（） 例如

    mWebView.loadUrl("javascript:callJS()");    

  2. 通过WebView的evaluateJavascript（）  

    mWebView.evaluateJavascript（"javascript:callJS()", new ValueCallback<String>() {    
    @Override    
    public void onReceiveValue(String value) {    
    //此处为 js 返回的结果    
    }    
    });    

#### 3.3.2 对于JS调用Android代码的方法有3种
  1. 通过WebView的addJavascriptInterface（）进行对象映射  

      >1. 定义一个与JS对象映射关系的Android类  
      public class AndroidtoJs extends Object {    
        // 定义JS需要调用的方法    
        // 被JS调用的方法必须加入@JavascriptInterface注解    
        @JavascriptInterface    
        public void hello(String msg) {    
        System.out.println("JS调用了Android的hello方法");    
        }    
        }    

      >2. 在html文件中定义一个调用Android功能的方法  
      function callAndroid(){    
        // 由于对象映射，所以调用test对象等于调用Android映射的对象    
        test.hello("js调用了android中的hello方法");    
        }    

      >3. 在Android里通过WebView设置Android类与JS代码的映射  
      // 设置与Js交互的权限    
      webSettings.setJavaScriptEnabled(true);    
      // 通过addJavascriptInterface()将Java对象映射到JS对象    
      //参数1：Javascript对象名    
      //参数2：Java对象名    
      mWebView.addJavascriptInterface(new AndroidtoJs(), "test");//AndroidtoJS类对象映射到js的test对象    

  2. 通过 WebViewClient 的shouldOverrideUrlLoading ()方法回调拦截 url  

      >1. 在JS约定所需要的Url协议  
      function callAndroid(){    
      //约定的url协议为：js://webview?arg1=111&arg2=222    
      document.location = "js://webview?arg1=111&arg2=222";    
      }    
      //点击按钮则调用callAndroid（）方法    
      onclick="callAndroid()"    

      >2. 在Android通过WebViewClient复写shouldOverrideUrlLoading()
      // 设置与Js交互的权限    
      webSettings.setJavaScriptEnabled(true);    
      mWebView.setWebViewClient(new WebViewClient() {    
      @Override    
      public boolean shouldOverrideUrlLoading(WebView view, String url) {    
      // 如果url的协议 = 预先约定的 js 协议    
      // 就解析往下解析参数    
      Uri uri = Uir.parse(url)    
      }    

  3. 通过 WebChromeClient的onJsAlert()、onJsConfirm()、onJsPrompt（）方法回调拦截JS对话框alert()、confirm()、prompt（） 消息  
    >1. 在JS约定所需要的Url协议  
    function callAndroid(){    
    //约定的url协议为：js://webview?arg1=111&arg2=222    
    document.location = "js://webview?arg1=111&arg2=222"      
    }    
    //点击按钮则调用callAndroid（）方法    
    onclick="callAndroid()"    

    >2.  WebChromeClient复写onJsPrompt()    
    //设置与Js交互的权限    
    webSettings.setJavaScriptEnabled(true);    
    mWebView.setWebChromeClient(new WebChromeClient() {    
    // 拦截输入框(原理同方式2)    
    @Override    
    public boolean onJsPrompt(WebView view, String url, String message, String defaultValue, JsPromptResult result) {    
    // 如果url的协议 = 预先约定的 js 协议    
    // 就解析往下解析参数    
    Uri uri = Uir.parse(message)    
    }    
    // 拦截JS的警告框    
    @Override    
    public boolean onJsAlert(WebView view, String url, String message, JsResult result) {    
    return super.onJsAlert(view, url, message, result);    
    }    
    // 拦截JS的确认框    
    @Override    
    public boolean onJsConfirm(WebView view, String url, String message, JsResult result) {    
    return super.onJsConfirm(view, url, message, result);    
    }    

### 3.4 [RecyclerView](https://blog.csdn.net/lmj623565791/article/details/45059587)
### 3.5 [ListView详细讲解](https://blog.csdn.net/guolin_blog/article/details/44996879)
### 3.6 ImageView
### 3.7 SurfaceView
### 3.8 GLSurfaceView
### 3.9 自定义view
### 3.10 自定义View、ViewGroup注意那些回调？
1. void onFinishInflate();  
    >当系统解析XML中声明的View后回调此方法，调用顺序：内层View->外层View,如果是viewgroup,适合在这里获取子View。  
    如果View没有在XML中声明而是直接在代码中构造的，则不会回调此方法
    此时无法获取到View的宽高和位置
2.  void onAttachedToWindow();  
    >当view 被添加到window中回调，调用顺序：外层View->内层View。在XML中声明或在代码中构造，并调用addview（this view）方法都会回调该方法。  
    此时View仅仅被添加到View，而没有开始绘制所以同样获取不到宽高和位置


### 3.11 Android中动画
[1. Frame Animation](https://blog.csdn.net/liuhe688/article/details/6657776)  
[2. Tween Animation](https://blog.csdn.net/liuhe688/article/details/6660823)  
[3. Layout Animation](https://blog.csdn.net/guolin_blog/article/details/43536355)  
[4. Property Animation](https://blog.csdn.net/guolin_blog/article/details/43536355)

***
## 4 UI原理
### 4.1 View，ViewGroup事件分发

***
## 5 Android屏幕适配
### 5.1 dp直接适配
  * dp是什么，sp呢，有什么区别
     dp:每英寸的像素点数，比如手机是1080x1920，手机的宽高是3x4英寸的，那么dp就是1080/3和1920/4 。
  * sp:除了受屏幕密度影响外,还受到用户的字体大小影响，通常情况下,建议使用sp来跟随用户字体大小设置。

### 5.2 宽高限定符适配
### 5.3 smallestWidth适配(sw限定符适配)
### 5.4 [UI适配框架](https://github.com/hongyangAndroid/AndroidAutoLayout)（已经停止维护）
### 5.5 [今日头条适配方案](https://zhuanlan.zhihu.com/p/37199709?utm_source=weibo&utm_medium=social&utm_oi=27871238160384)
***
## 6 性能优化相关
### 6.1 Android屏幕渲染机制
### 6.2 界面卡顿的原因以及解决方法
>一般人眼感觉卡顿的零界点是60FPS,所以为了让人眼感受不到卡顿，Android系统会每隔16ms发出VSYNC信号重新绘制界面。如果由于各种原因导致界面的刷新频率在16ms之后会出现掉帧的现象，这样会降低界面刷新的频率，导致用户感知到卡顿。主要原因有以下：
  >>1. 过于复杂的界面(使用页面复用include、ViewStub、merge等技术简化布局)
  >>2. 过度绘制(同上)
  >>3. UI线程处理过多任务(将复杂任务放入子线程中处理)
  >>4. 频繁的GC(代码优化)

***
## 7 持久化技术相关    

### 7.1 Share Preference
### 7.2 SQLite
### 7.3 ContentProvider
### 7.4 File
### 7.5 网络存储

***
## 8 Crash相关
### 8.1 Android中的ANR
### 8.2 [内存泄漏如何排查，MAT分析方法以及原理，各种泄漏的原因是什么比如](https://www.jianshu.com/p/ac00e370f83d)
传统的内存泄漏原因是：忘记释放分配的内存。逻辑内存溢出的原因是：当不再需要这个对象，还未释放该对象的所有引用。Android中的内存泄漏一般分为两种  
1. 全局进程的static变量，他无视应用状态，持有Activity的强引用。  
2. 活在activity生命周期之外的线程。

### 8.3  Android中最容易导致内存泄漏的是Context，还有以下情况：
1. Static Activities
2. Static View
3. Inner Class且有静态变量的引用
4. Anonymous Classes
5. Handler
6. Threads
7. TimerTask
8. Sensor Manager


***
## 9 网络、进程与线程
### 9.1 AsyncTask
### 9.2 [HttpClient与HttpUrlConnection的区别
](http://blog.csdn.net/guolin_blog/article/details/12452307 )
### 9.3 http与https的区别
### 9.4 进程保活（不死进程）
### 9.5 进程间通信的方式(IPC)
### 9.6 加载大图(ImageLoader)
### 9.7 Android消息机制
Android的消息机制一般指的是Handler的运行机制。Handler的运行机制离不开MessageQueue/Message/Looper/Handler这四个类。  
1. Message：消息产生分为硬件生成和软件生成。  
2. MessageQueue：主要功能是向消息池投递消息(MessageQueue.enqueueMessage)和取走消息(MessageQueue.next)。  
3. Handler：消息辅助类，主要功能向消息池发送各种消息事件(Handler.sendMessage)和处理相应消息事件(Handler.handleMessage)。  
4. Looper：不断循环执行(Looper.loop)，按分发机制将消息分发给目标处理者。

### Android 线程池的实现原理
### 讲解一下Context
### Java虚拟机和Dalvik虚拟机的区别
***
## 10 设计模式
***
## 11 开源框架
***
## 12 软件架构
