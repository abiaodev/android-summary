# Android问答
## 1、[Gradle各个依赖描述](https://blog.csdn.net/yuzhiqiang_1993/article/details/78366985)
- compile--->implementation,api
  > 这种是我们最常用的方式，使用该方式依赖的库将会参与编译和打包。

  - implementation
    > 该依赖方式所依赖的库不会传递，只会在当前module中生效。

  - api
    > 该依赖方式会传递所依赖的库，当其他module依赖了该module时，可以使用该module下使用api依赖的库。

- provided--->compileOnly
  > 只在编译时有效，不会参与打包

- apk--->runtimeOnly
  > 只在生成apk的时候参与打包，编译时不会参与，很少用。

- testCompile--->testImplementation
  > testCompile 只在单元测试代码的编译以及最终打包测试apk时有效。

- debugCompile--->debugImplementation
  > debugCompile 只在debug模式的编译和最终的debug apk打包时有效

- releaseCompile--->releaseImplementation
  > Release compile 仅仅针对Release 模式的编译和最终的Release apk打包。


## 2、onActivityresult不执行
- SingleTask
  > 原因：启动模式(launchMode)为SingleTask时，系统将在 startActivityForResult() 后直接调用 onActivityResult()。  
  > 解决：将启动模式切换为singleTop或standard

- Fragment 的onActivityResult() 不执行
  > 原因：启动Actity的时候，不是用Fragment启动  
  > 解决：使用fragment.startActivityForResult()或者在与Fragment绑定的FragmentActivity中重写onActivityResult方法。返回的result先传至Fragment的FragmentActiivity。这个FragementActivity的onActivityResult函数必须调用super(requestCode,resultCode, data); 。super.onActiivtyResult会将未被处理的result消息继续传给其fragement的onActiivtyResult。
如果这种方法不管用用以下这种：解决方法就是在Fragment中直接调用startActivityForResult()方法

## 3、[Android Studio突然导包无效 import无效 调用无效](https://blog.csdn.net/Mr_ChenXu/article/details/77987404)
- 步骤
  >Android studio file--->Invalidate Caches/Restart

## 4、怎么在app模块中引入其他模块的so文件，比如我的引用中有两个模块app和lib，我想要将百度地图的so文件提出来放到lib模块中，怎么在app模块引用到?


## 5、No toolchains found in the NDK toolchains folder for ABI with prefix: mips64el-linux-android

## 6、Android Studio 4以上xml中格式化代码组件重排序
1. 进入设置页(Settings)
2. Editor-->Code Style-->XML
3. 点击右上角set from 选择Android
4. Apply
