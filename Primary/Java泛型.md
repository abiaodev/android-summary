# 泛型
## 为什么设计泛型
因为我们不想要为聚集不同的类设计不同的代码（虽然可以用继承实现，但是继承不能对代码进行行错误检查），同时使代码容易阅读。简单来说就是泛型会使代码具有更高的安全性和可读性。
## 泛型类
引入一个类型变量T，使用<>括起来并放在类名后面（如下所示），泛型类可以有多个变量写法如：class Pair<U,S>{...}
```
class Pair<T> {
  private T data;
  public T getData(){
    return data;
  }
  public void setData(T data){
    this.data = data;
  }
}

public static void main(String[] args){
  Pair pair = new Pair<String>();
  pair.setData("this is data");
  System.out.println(pair.getData());
}
```
## 泛型方法
类型变量放在修饰符后面，返回类型之前（如下）。可以定义在普通类中也可以定义在泛型类中
```
class ArrayTool{
  public static <T> T getMiddleData(T... t){
      return t[t.length/2];
  }
}

public static void main(String[] args){
  ArrayTool.<String>getMiddleData("one","two","three");
  //以下是简写效果一样
  ArrayTool.getMiddleData("one","two","three");
}
```
## 类型变量的限定
有时候我们需要对类型变量加以约束，用于缩小类型变量的取值范围达到某些操作如compareTo。添加限定的方法是使用extends关键字，一个类型变量或通配符可以有多个限定写法如：T extends Comparable & Serializable
```
class ArrayTool{
  public static <T extends Comparable> T getMin(T... t){
    if(t == null||t.length<=0)return null;
    T min = t[0];
    for(int i=0;i<t.length;i++){
      if(min.compareTo(t[i])>0){
        min = t[i];
      }
    }
    return min;
  }
}
```
## 泛型代码和虚拟机
对于虚拟机来时所有的类都是普通类，是没有泛型类型对象的。所以无论何时定义了一个泛型类型，都会自动提供一个原始类型。原始类型就是删去类型参数后的泛型类型名，擦除类型变量，替换为原始类型变量。
如 class Pair<T> 的原始类型如下,因为T是个无限定的变量，所以直接用Object替换
```
class Pair {
  private Object data;
  public Object getData(){
    return data;
  }
  public void setData(Object data){
    this.data = data;
  }
}
```
如果使用了类型变量限定符
```
class Pair<T extends Comparable> {
  private T data;
  public T getData(){
    return data;
  }
  public void setData(T data){
    this.data = data;
  }
}
```
原始类型就变成如下
```
class Pair {
  private Comparable data;
  public Comparable getData(){
    return data;
  }
  public void setData(Comparable data){
    this.data = data;
  }
}
```
### 翻译泛型表达式
当程序调用泛型方法时，如果擦除返回类型，编译器插入强制类型转换。
