# Android基础面试题

## 1 Android系统的架构

### 1.1 应用层(System App)

系统内置应用与非内置应用，Java开发直接与用户交互。

### 1.2 应用框架层(Java API Framework)

为开发人员提供开发API，主要包含以下组件  

-   ActivityManager  
-   LocationManager  
-   PackageManager  
-   NotificationManager  
-   ResourceManager  
-   TelephonyManager  
-   WindowManager  
-   ContentManager  
-   ViewSystem  

### 1.3 系统运行库层(Native)

c/c++开发主要分为两个部分
1. c/c++程序库
2. Android运行时库

### 1.4 硬件抽象层(HAL)

运行在用户空间中，向下屏蔽硬件驱动模块的实现细节，向上提供硬件访问服务。

### 1.5 Linux内核

### 1.6 讲解一下Context

### 1.7 Java虚拟机和Dalvik虚拟机的区别

* * *

## 2  Android中四大组件

1.  Activity  
      Activity 表示具有用户界面的单一屏幕。例如，电子邮件应用可能具有一个显示新电子邮件列表的 Activity、一个用于撰写电子邮件的 Activity 以及一个用于阅读电子邮件的 Activity。 尽管这些 Activity 通过协作在电子邮件应用中形成了一种紧密结合的用户体验，但每一个 Activity 都独立于其他 Activity 而存在。因此，其他应用可以启动其中任何一个 Activity（如果电子邮件应用允许）。例如，相机应用可以启动电子邮件应用内用于撰写新电子邮件的 Activity，以便用户共享图片。
2.  Service  
      服务是一种在后台运行的组件，用于执行长时间运行的操作或为远程进程执行作业。 服务不提供用户界面。 例如，当用户位于其他应用中时，服务可能在后台播放音乐或者通过网络获取数据，但不会阻断用户与 Activity 的交互。诸如 Activity 等其他组件可以启动服务，让其运行或与其绑定以便与其进行交互。
3.  BroadcastReceiver  
      广播接收器是一种用于响应系统范围广播通知的组件。 许多广播都是由系统发起的 — 例如，通知屏幕已关闭、电池电量不足或已拍摄照片的广播。应用也可以发起广播 — 例如，通知其他应用某些数据已下载至设备，并且可供其使用。  
    尽管广播接收器不会显示用户界面，但它们可以创建状态栏通知，在发生广播事件时提醒用户。 但广播接收器更常见  
    的用途只是作为通向其他组件的“通道”，设计用于执行极少量的工作。 例如，它可能会基于事件发起一项服务来执行某项工作。
4.  ContentProvider  
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

-   STANDARD
       不管有没有已存在的实例，都生成新的实例。
-   SINGLETOP
       如果发现有对应的Activity实例正位于栈顶，则重复利用，不再生成新的实例。否则会重新生成新的实例。
-   SINGLETASK
       如果发现有对应的Activity实例，则使此Activity实例之上的其他Activity实例统统出栈，使此Activity实例成为栈顶对象，显示到幕前。
-   SINGLEINSTANCE
      启用一个新的栈结构，将Activity放置于这个新的栈结构中，并保证不再有其他Activity实例进入。

### 2.3 保存Activity状态

### 2.4 [Fragment的生命周期](https://blog.csdn.net/lmj623565791/article/details/37970961)

### 2.5 Service的[生命周期](https://blog.csdn.net/guolin_blog/article/details/11952435)以及[启动过程](http://gityuan.com/2016/03/06/start-service/)

### 2.6 Service的两种启动方式

#### 2.6.1 使用startService启动服务的生命周期方法

1.  使用这种方法启动一个Service，在Service启动后该Service和启动该Service的Activity就没有关系了。所以这种发放启动的Service不能和Activity进行交互。
2.  通常情况下使用startService调用的Service生命周期方法为：onCreate()->onStartCommand()。其中多次调用startService只调用一次onCreate(), 但可以多次调用onStartCommand()。
3.  当服务需要退出时，调用stopService，就会调用Service的onDestroy()方法。

#### 2.6.2 使用bindService启动服务的生命周期方法

1.  使用这种方法启动的Service是和调用者Activtiy同生命的，当Activtiy退出时，服务也同时销毁了。这种方法启动的Service能够和Activity进行交互。
2.  调用bindService启动服务，Service生命周期方法为：onCreate()->onBind()多次调用bindService并不会多次调用onBind(),即onCreate()和onBind()都是只被调用一次。
3.  当Activity退出时，该Service销毁，调用：onUnbind()->onDestroy();

### 2.7 [Broadcast注册方式与区别](https://blog.csdn.net/carson_ho/article/details/52973504)

静态注册是常驻广播，不受任何组件生命周期的影响，只有在时刻监听广播的时候使用。缺点是耗电占内存。  动态注册则比较灵活可以控制其生命周期，在特定的情况下监听使用。

#### 2.7.1 静态注册(minifest)

在AndroidManifest.xml里通过标签声明,当此App首次启动时，系统会自动实例化mBroadcastReceiver类，并注册到系统中。

#### 2.7.2 动态注册

在代码中通过调用Context的registerReceiver（）方法进行动态注册BroadcastReceiver,动态广播最好在Activity的onResume()注册、onPause()注销。

* * *

## 3 UI相关(使用)

### 3.1 Android中常用布局

#### 3.1.1 LinearLayout

#### 3.1.2 TableLayout

#### 3.1.3 RelativeLayout

#### 3.1.4 FrameLayout

#### 3.1.5 AbsoluteLayout

#### 3.1.6 GridLayout

#### 3.1.7 CoordinatorLayout

### 3.2 [WebView](https://www.jianshu.com/p/fd61e8f4049e)及其常用方法

-   onPause() 尽力尝试暂停可以暂停的任何处理，如动画和地理位置。 不会暂停JavaScript。 要全局暂停JavaScript，可使用pauseTimers。
-   onResume() 恢复onPause() 停掉的操作；
-   pauseTimers() 暂停所有WebView的布局，解析和JavaScript定时器。 这个是一个全局请求，不仅限于这个WebView。
-   resumeTimers() 恢复所有WebView的所有布局，解析和JavaScript计时器，将恢复调度所有计时器。

#### 3.2.1 [WebView与js交互](https://blog.csdn.net/carson_ho/article/details/64904691/)

##### 3.2.1.1 对于Android调用JS代码的方法有2种

1.  通过WebView的loadUrl() 例如:    

        mWebView.loadUrl("javascript:callJS()");

2.  通过WebView的evaluateJavascript()  

        mWebView.evaluateJavascript（"javascript:callJS()", new ValueCallback<String>() {    
              @Override    
              public void onReceiveValue(String value) {    
                  //此处为 js 返回的结果    
              }    
          });    

##### 3.2.1.2 对于JS调用Android代码的方法有3种

1.  通过WebView的addJavascriptInterface（）进行对象映射  

    1.  定义一个与JS对象映射关系的Android类  

            public class AndroidtoJs extends Object {    
                // 定义JS需要调用的方法    
                // 被JS调用的方法必须加入@JavascriptInterface注解    
                @JavascriptInterface    
                public void hello(String msg) {    
                    System.out.println("JS调用了Android的hello方法");    
                }    
            }    

    2.  在html文件中定义一个调用Android功能的方法  

            function callAndroid(){    
                // 由于对象映射，所以调用test对象等于调用Android映射的对象    
                test.hello("js调用了android中的hello方法");    
            }    

    3.  在Android里通过WebView设置Android类与JS代码的映射  

            // 设置与Js交互的权限    
            webSettings.setJavaScriptEnabled(true);    
            // 通过addJavascriptInterface()将Java对象映射到JS对象    
            //参数1：Javascript对象名    
            //参数2：Java对象名    
            mWebView.addJavascriptInterface(new AndroidtoJs(), "test");

2.  通过 WebViewClient 的shouldOverrideUrlLoading ()方法回调拦截 url  

    1.  在JS约定所需要的Url协议  

            function callAndroid(){    
                //约定的url协议为：js://webview?arg1=111&arg2=222    
                document.location = "js://webview?arg1=111&arg2=222";    
            }    
            //点击按钮则调用callAndroid（）方法    
            onclick="callAndroid()"    

    2.  在Android通过WebViewClient复写shouldOverrideUrlLoading()

            // 设置与Js交互的权限    
            webSettings.setJavaScriptEnabled(true);    
            mWebView.setWebViewClient(new WebViewClient() {    
                @Override    
                public boolean shouldOverrideUrlLoading(WebView view, String url) {    
                    // 如果url的协议 = 预先约定的 js 协议    
                    // 就解析往下解析参数    
                    Uri uri = Uir.parse(url)    
                }    

           }

3.  通过 WebChromeClient的onJsAlert()、onJsConfirm()、onJsPrompt（）方法回调拦截JS对话框alert()、confirm()、prompt（） 消息  
          1. 在JS约定所需要的Url协议    

                  function callAndroid(){    
                      //约定的url协议为：js://webview?arg1=111&arg2=222    
                      document.location = "js://webview?arg1=111&arg2=222"    
                  }    
                  //点击按钮则调用callAndroid（）方法    
                  onclick="callAndroid()"    

          2.  WebChromeClient复写onJsPrompt()  

                  //设置与Js交互的权限    
                  webSettings.setJavaScriptEnabled(true);    
                  mWebView.setWebChromeClient(new WebChromeClient() {    
                      // 拦截输入框(原理同方式2)    
                      @Override    
                      public boolean onJsPrompt(WebView view, String url, String message, String defaultValue, JsPromptResult result) {
                            // 如果url的协议 = 预先约定的 js 协议    
                            // 就解析往下解析参数    
                            Uri uri = Uir.parse(message);   
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
                    }

    ### 3.3 TextView

    ### 3.4 [RecyclerView](https://blog.csdn.net/lmj623565791/article/details/45059587)

    ### 3.5 [ListView](https://blog.csdn.net/guolin_blog/article/details/44996879)

    ### 3.6 ImageView

    ### 3.7 SurfaceView

    ### 3.8 GLSurfaceView

    ### 3.9 自定义view

    #### 3.9.1 自定义View、ViewGroup注意那些回调？

4.  void onFinishInflate();  
    > 当系统解析XML中声明的View后回调此方法，调用顺序：内层View->外层View,如果是viewgroup,适合在这里获取子View。  
    > 如果View没有在XML中声明而是直接在代码中构造的，则不会回调此方法
    > 此时无法获取到View的宽高和位置
5.  void onAttachedToWindow();  
    > 当view 被添加到window中回调，调用顺序：外层View->内层View。在XML中声明或在代码中构造，并调用addview（this view）方法都会回调该方法。  
    > 此时View仅仅被添加到View，而没有开始绘制所以同样获取不到宽高和位置

### 3.10 Android中动画

[1. Frame Animation](https://blog.csdn.net/liuhe688/article/details/6657776)  
[2. Tween Animation](https://blog.csdn.net/liuhe688/article/details/6660823)  
[3. Layout Animation](https://blog.csdn.net/guolin_blog/article/details/43536355)  
[4. Property Animation](https://blog.csdn.net/guolin_blog/article/details/43536355)

* * *

## 4 UI原理

### 4.1 View、ViewGroup事件分发

#### 4.1.1 MotionEvent

当用户点击屏幕的时候会触发以下一系列的事件，常见的事件如下：  

> 1.  ACTION_DOWN------手指接触屏幕
> 2.  ACTION_MOVE-------手指在屏幕是移动
> 3.  ACTION_UP------------手指离开屏幕

可以通过以下方法获得当前手指坐标：  

> 1.  getX()、getY()-----------------获取相对于当前View的X,Y坐标
> 2.  getRawX()、getRawY()------获取相对于当前屏幕的X,Y坐标

#### 4.1.2 点击事件的传递规则

1.  当系统产生一个MotionEvent之后，要将其传递给一个具体的View的过程就是事件传递的过程。这个过程需要用到以下三个重要的方法。

    > 1.  public boolean dispatchTouchEvent(MotionEvent me)--------事件传递到的View的这个方法一定会调用，返回结果当前时间是否被消耗受2，3结果影响。
    > 2.  public boolean onInterceptTouchEvent(MotionEvent me)--------在1内调用，返回是否拦截事件，如果拦截，在当前View的事件序列中该方法不会再调用。
    > 3.  public boolean onTouchEvent(MotionEvent me)--------在1中调用，用来处理事件，返回是否消耗当前事件。如果不消耗，当前View将不会再接收该事件序列中的事件。

2.  三个方法的关系如以下伪代码    

        public boolean dispatchTouchEvent(MotionEvent me){    
            boolean consume = false;    
            if(onInterceptTouchEvent(me)){    
                consume = onTouchEvent(me);    
            }else{    
                consume = dispatchTouchEvent(me);    
            }    
            return consume;
        }

* * *

## 5 Android屏幕适配

### 5.1 dp直接适配

-   dp是什么，sp呢，有什么区别
     dp:每英寸的像素点数，比如手机是1080x1920，手机的宽高是3x4英寸的，那么dp就是1080/3和1920/4 。
-   sp:除了受屏幕密度影响外,还受到用户的字体大小影响，通常情况下,建议使用sp来跟随用户字体大小设置。

### 5.2 宽高限定符适配

### 5.3 smallestWidth适配(sw限定符适配)

### 5.4 [UI适配框架](https://github.com/hongyangAndroid/AndroidAutoLayout)（已经停止维护）

### 5.5 [今日头条适配方案](https://zhuanlan.zhihu.com/p/37199709?utm_source=weibo&utm_medium=social&utm_oi=27871238160384)

* * *

## 7 持久化技术相关

### 7.1 [SharePreference](https://www.jianshu.com/p/59b266c644f3)

一个轻量级的存储类，特别适合用于保存软件配置参数。（是用xml文件存放数据，文件存放在/data/data/<package name>/shared_prefs目录下）,使用步骤：
1. 使用Activity类的getSharedPreferences方法获得SharedPreferences对象；

        SharedPreferences sharedPreferences = getSharedPreferences(String name, int mode)

2.  使用SharedPreferences接口的edit获得SharedPreferences.Editor对象；

        Editor editor = sharedPreferences.edit();

3.  通过SharedPreferences.Editor接口的putXXX方法保存key-value对；

        editor.putString(KEY, VALUES);

4.  通过过SharedPreferences.Editor接口的commit方法保存key-value对。

        editor.commit();

5.  通过SharedPreferences对象的getXXX方法获取数据；

        Object values = sharedPreferences.getXXX(KEY, DEFAULT_VALUE);

### 7.2 [SQLite](https://blog.csdn.net/column/details/android-database-pro.html)

SQLite传统使用步骤
1. 新建一个MySQLiteHelper类并让它继承SQLiteOpenHelper

        public class MySQLiteHelper extends SQLiteOpenHelper {
            public static final String CREATE_NEWS = "create table news ("
                + "id integer primary key autoincrement, "
                + "title text, "
                + "content text, "
                + "publishdate integer,"
                + "commentcount integer)";
            public MySQLiteHelper(Context context, String name, CursorFactory factory,
        int version) {
                super(context, name, factory, version);
            }
            @Override
            public void onCreate(SQLiteDatabase db) {
                db.execSQL(CREATE_NEWS);
            }
            @Override
            public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
            }
        }

2.  创建SQLiteDatabase的实例，数据库会自动创建，如下所示

        SQLiteOpenHelper dbHelper = new MySQLiteHelper(this, "demo.db", null, 1);
        SQLiteDatabase db = dbHelper.getWritableDatabase();

### 7.3 ContentProvider

### 7.4 File

### 7.5 网络存储

* * *

## 8 Crash相关

### 8.1 Android中的ANR

### 8.2 [内存泄漏如何排查，MAT分析方法以及原理，各种泄漏的原因是什么比如](https://www.jianshu.com/p/ac00e370f83d)

传统的内存泄漏原因是：忘记释放分配的内存。逻辑内存溢出的原因是：当不再需要这个对象，还未释放该对象的所有引用。Android中的内存泄漏一般分为两种  
1. 全局进程的static变量，他无视应用状态，持有Activity的强引用。  
2. 活在activity生命周期之外的线程。

### 8.3  Android中最容易导致内存泄漏的是Context，还有以下情况：

1.  Static Activities
2.  Static View
3.  Inner Class且有静态变量的引用
4.  Anonymous Classes
5.  Handler
6.  Threads
7.  TimerTask
8.  Sensor Manager

* * *

## 9 进程与线程

### 9.1 进程保活（不死进程）

### 9.2 进程间通信的方式(IPC)

### 9.3 Android消息机制

Android的消息机制一般指的是Handler的运行机制。Handler的运行机制离不开MessageQueue/Message/Looper/Handler这四个类。  
1. Message---------------消息产生分为硬件生成和软件生成。  
2. MessageQueue--------主要功能是向消息池投递消息(MessageQueue.enqueueMessage)和取走消息(MessageQueue.next)。  
3. Handler----------------消息辅助类，主要功能向消息池发送各种消息事件(Handler.sendMessage)和处理相应消息事件(Handler.handleMessage)。  
4. Looper-----------------不断循环执行(Looper.loop)，按分发机制将消息分发给目标处理者。

具体过程如下：  
1. 在一个有Looper的线程中创建Handler之后，Handler就可以和其内部的MessageQueue和Looper一起运转。  
2. 通过handler的post(new Runnable)或者send(new Message)方法去调用MessageQueue的enqueueMessage方法将消息放入MessageQueue。  
3. 当Looper发现新消息到来时，就会处理这个消息，就是消息中的Runnable或者handler的handleMessage()会执行。因为Looper是存在于创建Handler的线程中，所以处理消息是在创建Handler的那个线程中。

### 9.4 Android线程

#### 9.4.1 AsyncTask

AsyncTask是一个轻量级的异步任务类，他可以在线程池中执行后台任务，并将执行的进度和结果传递给主线程。AsyncTask是一个抽象的泛型类，是对Thread和Handler的轻量级封装。他提供了以下四个核心方法：
1. onPreExecute()------------------------------在主线程中执行，在异步任务执行之前，用于准备工作。
2. doInBackGround(Params...params)------在线程池中执行，params表示异步任务的输入参数。再次方法中可以调用publishProgress()来更新任务进度，次方法会调用onProgressUpdate(),而且次方法需要返回计算结果给onPostExecute()。
3. onProgressUpdate(progress...values)----在主线程中执行，当后台任务执行进度发生改变时调用。
4. onPostExecute(Result result)-------------在主线程中执行，异步任务完成之后，result是doInBackground的值。

使用步骤
1. 创建一个类MyAsyncTask继承AsyncTask&lt;Void,Integer,Integer>并重写以上三个方法。
2. 创建实例，并执行  
MyAsyncTask myAsyncTask = new MyAsyncTask(this);      myAsyncTask.execute(参数);    

#### 9.4.2 HandlerThread

HandlerThread能够新建拥有Looper的线程。这个Looper能够用来新建其他的Handler。主要用途是用于会长时间在后台运行，并且间隔时间内（或适当情况下）会调用的情况，例子：实现IntentService或实时更新等，以下是使用步骤
1. 创建HandlerThread实例:  

            mHandlerThread = new HandlerThread("check-message-coming");    
            mHandlerThread.start();    

2.  创建并初始化主线程的Handler:  

            mainThreadHandler = new Handler();    

3.  通过HandlerThread创建并初始化子线程的Handler:   

            subThreadHandler = new Handler(mHandlerThread.getLooper()){  
                @Override
                public void handleMessage(Message msg){  
                    //模拟数据更新    
                    mainThreadHandler.post(new Runnable(){    
                      @Override    
                      public void run(){     
                        //更新主线程UI    
                      }
              });    
              if (isUpdateInfo){    
                  //mainThreadHandler继续    
                  subThreadHandler.sendEmptyMessage(MSG_UPDATE_INFO);
                }
            }

4.  在页面开始的时候通知执行，暂停的时候通知停止，销毁的时候释放HandlerThread

            @Override    
            protected void onResume()    
            {    
                super.onResume();    
                //开始查询    
                isUpdateInfo = true;    
                subThreadHandler.sendEmptyMessage(MSG_UPDATE_INFO);    
            }    
            @Override
            protected void onPause()
            {
                super.onPause();
                //停止查询
                //以防退出界面后Handler还在执行
                isUpdateInfo = false;
                subThreadHandler.removeMessages(MSG_UPDATE_INFO);
            }
            @Override
            protected void onDestroy()
            {
                super.onDestroy();
                //释放资源
                mHandlerThread.quit();
            }

#### 9.4.3 IntentService

IntentService，可以看做是Service和HandlerThread的结合体，在完成了使命之后会自动停止，适合需要在工作线程处理UI无关任务的场景。
    1. IntentService 是继承自 Service 并处理异步请求的一个类，在 IntentService 内有一个工作线程来处理耗时操作。
    2. 当任务执行完后，IntentService 会自动停止，不需要我们去手动结束。
    3. 如果启动 IntentService 多次，那么每一个耗时操作会以工作队列的方式在 IntentService 的 onHandleIntent 回调方法中执行，依次去执行，使用串行的方式，执行完自动结束。
使用步骤
1. 创建一个Server继承IntentService例如：    

          public class MyIntentService extends IntentService{
              private static final String TAG = "TAG_MyIntentService";
              /**
                 * Creates an IntentService.  Invoked by your subclass's constructor.
                 *
                 * @param name Used to name the worker thread, important only for debugging.
                 */
                public MyIntentService(String name) {
                      super(name);
                }
                public MyIntentService(){
                    super(TAG);
                }
                @Override
                protected void onHandleIntent(@Nullable Intent intent) {
                    //执行异步任务
                    String action = intent.getStringExtra("task_action");
                    if (action.equals("com.intent.biao.task1")){
                        Log.d(TAG,"onHandleIntent com.intent.biao.task1");
                    }
                }

                @Override
                public void onDestroy() {
                    super.onDestroy();
                    Log.d(TAG,"MyIntentService destroy");
                }
            }

2.  在manifest中注册MyIntentService  
3.  通过Intent启动MyIntentService  

            Intent intent = new Intent(MainActivity.this,MyIntentService.class);
            intent.putExtra("task_action","com.intent.biao.task1");
            startService(intent);

### 9.5 Android 线程池的实现原理

线程池的优点：  
1. 复用线程池中的线程，俭省了线程的创建和销毁带来的开销。
2. 有效的控制线程并发数，避免因为线程并发过多导致抢占系统资源而阻塞。
3. 可以对线程简单的管理，提供定时执行和指定间隔循环执行等。

#### 9.5.1 ThreadPoolExecutor(线程池的真正实现)

常用构造方法：   

    public ThreadPoolExecutor(int corePoolSize, int maximumPoolSize, long keepAliveTime, TimeUnit unit, BlockingQueue<Runnable> workQueue, ThreadFactory threadFactory)

1.  corePoolSize：核心线程数，默认情况下会一直存活，即使处于闲置状态。如果将ThreadPoolExecutot的allowCoreThreadTimeOut属性设置为true，闲置的核心线程在等待任务到来时会有超时策略，由keepAliveTime控制时间间隔，超过这个时间闲置核心线程将被终止。
2.  maximumPoolSize：允许的最大线程数，超过之后新到来的任务将被阻塞。
3.  keepAliveTime：非核心线程的闲置超时时常，当ThreadPoolExecutot的allowCoreThreadTimeOut属性设置为true时，可以作用于核心线程。
4.  unit：keepAliveTime的时间单位，常用的有TimeUnit.MILLISECONDS(毫秒)、TimeUnit.SECONDS(秒)、TimeUnit.MINUTES(分钟)等。
5.  workQueue：线程池中的任务队列，通过ThreadPoolExecutor的execute()提交的Runnable对象会存储在这个队列中。
6.  threadFactory：线程工厂，为线程池提供创建新线程的功能，他是一个接口，只有一个方法，Thread newThread(Runnable runnable)。

#### 9.5.2 Android中常见的四类线程池

1.  FixedThreadPool  
    线程数固定的线程池，通过Executors.newFixedThreadPool()创建，所有的线程都是核心线程，并且没有超时策略，所以它能够很快的响应外界的请求。当所有的线程都处于活动状态时，新任务会处于等待状态直到有线程空闲出来。以下是其实现：  

            new ThreadPoolExecutor(nThreads, nThreads, 0L, TimeUnit.MILLISECONDS, new LinkedBlockingQueue<Runnable>());

2.  CachedThreadPool  
    线程数量不定，通过Executors.newCachedThreadPool()创建，所有的线程都是非核心线程，最大线程数为Integer.MAX_VALUE，有60秒的延时策略，当有新任务到来时，如果有闲置线程用闲置线程处理，如果没有则创建新线程处理，适合用于大量的耗时少的任务。以下是具体实现：

            new ThreadPoolExecutor(0, Integer.MAX_VALUE, 60L, TimeUnit.SECONDS, new SynchronousQueue<Runnable>());

3.  ScheduledThreadPool  
    核心线程数固定，非核心线程数无限制，通过Executors.newScheduledThreadPool()创建，非核心线程闲置立即被回收，主要用于执行定时任务和固定周期的重复任务。以下是具体实现：

            public ScheduledThreadPoolExecutor(int corePoolSize) {
                super(corePoolSize, Integer.MAX_VALUE,  DEFAULT_KEEPALIVE_MILLIS, MILLISECONDS,new DelayedWorkQueue());
            }

4.  SingleThreadExecutor
    只有一个核心线程，通过Executors.newSingleThreadExecutor()创建，用于将所有外界任务统一到一个线程来执行。以下是具体实现

            new FinalizableDelegatedExecutorService (
              new ThreadPoolExecutor(1, 1, 0L, TimeUnit.MILLISECONDS, new LinkedBlockingQueue<Runnable>())
            );

* * *

## 10 网络

### 10.1 [HttpClient与HttpUrlConnection的区别](http://blog.csdn.net/guolin_blog/article/details/12452307)

### 10.2 http与https的区别

1.  https协议需要到ca申请证书，一般免费证书较少，因而需要一定费用。
2.  http是超文本传输协议，信息是明文传输，https则是具有安全性的ssl加密传输协议。
3.  http和https使用的是完全不同的连接方式，用的端口也不一样，前者是80，后者是443。
4.  http的连接很简单，是无状态的；HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，比http协议安全
    ### 10.3 推送原理
5.  确定使用的传输协议(XMPP)
6.  开启新线程启动后台任务
7.  启动一个Service用于通讯
8.  在service的start(),方法中注册推送消息广播和通信连接的广播，用于监听消息是否到来。

* * *

## 11 开源框架

* * *

## 12 软件架构

* * *

## 13 打包

* * *

### 13.1 ProGuard技术
