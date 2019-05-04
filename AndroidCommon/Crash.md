
# AndroidCrash相关

## Android中的ANR

## [内存泄漏如何排查，MAT分析方法以及原理，各种泄漏的原因是什么比如](https://www.jianshu.com/p/ac00e370f83d)
  > 传统的内存泄漏原因是：忘记释放分配的内存。逻辑内存溢出的原因是：当不再需要这个对象，还未释放该对象的所有引用。Android中的内存泄漏一般分为两种  

  - 全局进程的static变量，他无视应用状态，持有Activity的强引用。  

  - 活在activity生命周期之外的线程。

## Android中最容易导致内存泄漏的是Context，还有以下情况：
  1.  Static Activities

  2.  Static View

  3.  Inner Class且有静态变量的引用

  4.  Anonymous Classes

  5.  Handler

  6.  Threads

  7.  TimerTask

  8.  Sensor Manager
