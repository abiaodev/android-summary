# Android开发常见问题总结

## [AppTheme冲突](http://yifeng.studio/2017/07/09/android-manifest-merge-conflict/)
- 总结
  - App 的主题名字最好具有唯一性， 不要叫 AppTheme，容易同名。
  - App 的主题要定义完整，比如 values / values-21
  - AndroidManifest.xml 文件中 application 节点配置如下:  
      ```
      <application  
      android:allowBackup="false"  
      android:icon="@mipmap/app_icon"  
      android:label="@string/app_name"  
      android:theme="@style/AppTheme"  
      tools:replace="android:theme"//添加这一行
      />
      ```
## [自定义RadioButton](https://blog.csdn.net/coder_pig/article/details/47035625)
1. 创建drawable文件例如：radio_button_1.xml，图片大小不好控制所以我使用drawable控制图片大小防止失真。
      ```
      <selector xmlns:android="http://schemas.android.com/apk/res/android">   
       <item  
        android:drawable="@drawable/radio_button_pressed"  
        android:state_checked="true"  
        android:state_enabled="true"
        />  
      <item  
        android:drawable="@drawable/radio_button_unpressed"  
        android:state_checked="false"  
        android:state_enabled="true"  
        />  
      </selector>
      ```
2. 在布局文件中这样使用,设置文字和button之间的距离
      ```
      <RadioButton  
          android:text="否"  
          android:textSize="14dp"  
          android:paddingLeft="8dp"<!--设置文字和button之间的距离--\>    
          android:button="@drawable/radio_button_1"/>
      ```
## [ToolBar](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2014/1118/2006.html)
1. 左侧/右侧有一段空白无法使用
      ```
      <android.support.v7.widget.Toolbar  
        android:layout_width="match_parent"  
        android:layout_height="?android:actionBarSize"  
        app:contentInsetStart="0dp"//左边  
        app:contentInsetEnd="0dp"右边
        app:contentInsetLeft="0dp"  
        app:contentInsetRight="0dp">  
        <!-- 自定义的布局--\>  
        <View  
            android:layout_width="match_parent"  
            android:layout_height="match_parent"  
            android:background="@android:color/holo_green_dark"/>
      </android.support.v7.widget.Toolbar>
      ```

## Android Studio获取SHA1值
  > 调试版本（debug）和发布版本（release）下的 SHA1 值是不同的，发布 apk 时需要根据发布 apk 对应的 keystore 重新配置 Key。(注意：我们这里使用的是调试版本,在开发时请使用调试版本) 。   

- 使用keytool（Android Studio + Windows场景）

    1. 运行进入控制台（）  

    2. 定位到.android文件夹下，输入
        > cd .android

    3. 调试版本使用debug.keystore，命令为。
        - debug
          > keytool -list -v -keystore debug.keystore

        - release
          > keytool -list -v -keystore apk 的 keystore

    4. 输入密钥库密码，调试版本默认密码是: android，发布模式的密码是apk的keystore设置的密码。输入密钥后回车（如果没设置密码，可直接回车），    
    此时可在控制台显示的信息中获取SHA1值.

- 在eclipse中，在adt 22中直接查看
    - 如果使用adt 22，可以在eclipse中直接查看
      > Windows -> Preferences -> android -> build。

## Android Studio模板
1. 方法的注释模板
    ```
    *
     * @description:
     * @author Abiao
     * @date $date$ $time$
    $params$
     * @return $returns$
    */
    ```

2. 文件注释模板
  ```
  /**
   * @author ${USER}_Abiao
   * @date ${DATE} ${TIME}
   * @description:
   */
  ```
