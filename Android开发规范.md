# 39医生互联网医院Android开发手册
为提高开发质量，规范团队开发，增强代码可读性，降低项目维护成本,特制定以下开发规范用于团队开发。
##  一、	Android Studio（以下简称AS）规范

1. AS版本最新稳定版本。
2. 统一使用UTF-8格式。
3. 编码风格使用AS默认风格，编辑完成之后提交之前必须格式化。
4. 提交的代码必须是可运行的程序。
5. 删除多余的import以减少警告。
6. 创建类的时候使用以下模板
  ```
  /**
   * @author developer
   * @date ${DATE} ${TIME}
   * @description:
   */
  ```

## 二、	包结构


## 三、	命名规范

### 1 资源文件命名与使用
1. layout文件命名

|       Layout         |规则|
|:------------------:|-----------------|
|Activity         | module_act_开头 |
|Fragment         | module_fg_开头 |
|Dialog         | module_dialog_开头 |
|include         | module_include_开头 |
|ListView         | module_list_item_开头 |
|RecyclerView         | module_recycle_item_开头 |
|GridView         | module_grid_item_开头 |

2. id资源命名以所在的layout开头，功能居中，View的缩写为后缀（例如act_doctor_details_consult_btn）

|       控件         |缩写|
|:------------------:|-----------------|
|LinearLayout         | ll |
|RelativeLayout         | rl |
|ConstraintLayout         | cl |
|ListView         | lv |
|ScollView         | sv |
|NestedScrollView         | nsv |
|SmartRefreshLayout         | srl |
|TextView         | tv |
|Button         | btn |
|ImageView         | iv |
|RadioButton         | rb |
|CheckBox         | cb |
|EditText         | et |

3. drawable 资源名称以小写单词+下划线的方式命名，根据分辨率不同存放
在不同的 drawable 目录下,命名规则如下:
```
模块名_业务功能描述_控件描述_控件状态限定词
```

4. anim 资源名称以小写单词+下划线的方式命名，采用以下规则：

|       名称         |说明|
|:------------------:|-----------------|
|fade_in         | 淡入 |
|fade_out         | 淡出 |
|push_down_in         | 从下方推入 |
|push_down_out         | 从下方推出 |
|push_left         | 推向左方 |
|slide_in_from_top         | 从头部滑动进入 |
|zoom_enter         | 变形进入 |
|slide_in         | 滑动进入 |
|shrink_to_middle         | 中间缩小 |
```
模块名_逻辑名称_[方向|序号]
```


5. color 资源使用#AARRGGBB 格式，写入 module_colors.xml 文件中，命
名格式采用以下规则：
```
模块名_逻辑名称_颜色
```

6. string资源文件或者文本用到字符需要全部写入module_strings.xml文件中，
字符串以小写单词+下划线的方式命名，采用以下规则：
```
模块名_逻辑名称
```

### 组件名
所有的Activity直接或者间接必须继承自BaseActivity
所有的Fragment直接或者间接必须继承自BaseFragment

|       类	|描述	|例如|
|:------------------:|-----------------|---------:|
|Activity         | Activity后缀 | LoginActivity|
|Adapter         | Adapter为后缀 | MessageAdapter|
|解析类         | Parser 为后缀 | HomeResultParser|
|工具类         | Utils/Manager/Helper为后缀 | LogUtils/AppAlarmManager/AudioHelper|
|数据库类         | DBHelper后缀 | NewsDBHelper|
|自定义的共享基础类         | Base为前缀 | BaseFragemnt/BaseActivity/BaseDialog|
### 包名
全部使用小写字母，不能使用下划线，采用反域名命名规则。
```
一级包名为顶级域名
com、edu、gov、net、org等
二级包名为公司名称
tencent 等
三级包名为应用名相关
weichat 等
之后的就是功能相关
activits 等
例如
com.tencent.weichat.activits
```


### Java编码规则（更多参考**阿里巴巴 Java 开发手册**）
1. 代码中的命名严禁使用拼音与英文混合的方式，更不允许直接使用中文的方式。
2. 类名必须使用 UpperCamelCase 风格，但以下情形例外： DO / BO / DTO / VO / AO /
PO / UID 等。
3. 方法名、参数名、成员变量、局部变量都统一使用 lowerCamelCase 风格，必须遵从
驼峰形式。
4. 常量命名全部大写，单词间用下划线隔开，力求语义表达完整清楚，不要嫌名字长。
5. 抽象类命名使用 Abstract 或 Base 开头； 异常类命名使用 Exception 结尾； 测试类
命名以它要测试的类的名称开始，以 Test 结尾。
6. 接口类中的方法和属性不要加任何修饰符号（public 也不要加） ，保持代码的简洁
性，并加上有效的 Javadoc 注释。尽量不要在接口里定义变量，如果一定要定义变量，肯定是
与接口方法相关，并且是整个应用的基础常量
7. 不允许任何魔法值（即未经预先定义的常量） 直接出现在代码中
8. 单个方法的总行数不超过 80 行

### 代码样式规范

1. 字符串常量命名

|       类	|字段前缀|
|:------------------:|-----------------|
|SharedPreferences         | SP_ |
|Bundle         | BUNDULE_ |
|Fragment Arguments         | ARGUMENT_或者KEY_TO_ |
|Intent Extra         | EXTRA_或KEY_TO_ |
|Intent Action         | ACTION_ |

2. Activit和Fragment传参

```
Activity
//可以在当前activity当中使用Android Studio 的Live Templates 功能**starter**快速生成。
private static String EXTRA_USER = "_EXTRA_USER_";
public static void start(Context context, User user) {
      Intent intent = new Intent(context, MainActivity.class);
      intent.putParcelableExtra(EXTRA_USER, user);
      context.startActivity(starter);
}

Fragment
//可以在当前activity当中使用Android Studio 的Live Templates 功能**newInstance**快速生成。
private static String ARGUMENT_USER = "_ARGUMENT_USER_";
public static MainFragment newInstance(User user) {
      Bundle args = new Bundle();
      args.putParcelable(ARGUMENT_USER, user);
      MainFragment fragment = new MainFragment();
      fragment.setArguments(args);
      return fragment;
}
```
