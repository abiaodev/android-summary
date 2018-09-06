# Android开发常见问题总结
##1 UI相关
###1 [AppTheme冲突](http://yifeng.studio/2017/07/09/android-manifest-merge-conflict/)
1. App 的主题名字最好具有唯一性， 不要叫 AppTheme，容易同名。
2. App 的主题要定义完整，比如 values / values-21
3. AndroidManifest.xml 文件中 application 节点配置如下:  
      >application  
      android:allowBackup="false"  
      android:icon="@mipmap/app_icon"  
      android:label="@string/app_name"  
      android:theme="@style/AppTheme"  
      tools:replace="android:theme"//添加这一行

###2 [RadioButton](https://blog.csdn.net/coder_pig/article/details/47035625)自定义
1. 创建drawable文件例如：radio_button_1.xml，图片大小不好控制所以我使用drawable控制图片大小防止失真。
      >\<selector xmlns:android="http://schemas.android.com/apk/res/android">   
       \<item  
        android:drawable="@drawable/radio_button_pressed"  
        android:state_checked="true"  
        android:state_enabled="true"
        />  
      \<item  
        android:drawable="@drawable/radio_button_unpressed"  
        android:state_checked="false"  
        android:state_enabled="true"  
        />  
      \</selector>

2. 在布局文件中这样使用,设置文字和button之间的距离
      > \<RadioButton  
                android:text="否"  
                android:textSize="14dp"  
                android:paddingLeft="8dp"//设置文字和button之间的距离  
                android:button="@drawable/radio_button_1"
                />
