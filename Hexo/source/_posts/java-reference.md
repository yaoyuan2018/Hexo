---
title: Java的4种引用类型
date: 2019-04-08 23:29:58
tags:
  -java
categories:
  -后端
---
## Java的4中引用类型
---
#### 01  概述
&emsp;在Java中提供了四个级别的引用：
  1. **强引用**
  2. **软引用**
  3. **弱引用**
  4. **虚引用**

&emsp;在这四个引用类型中，只有强引用`FinalReference`类是包内可见，其他三种引用类型均为public，可以在应用程序中直接使用。引用类型的结构如图所示：

![四种引用](https://mmbiz.qpic.cn/mmbiz_png/g1iakl3kULHBiaUbd5yMicObIuwhehZibAbspndVvAWqvNgR89U3rN3Ha66VzpD0YPPicggcqFv3jibUDBbMMNbFscnw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)


---
#### 02 强引用

&emsp;Java中的引用，类似C语言中最难的指针。通过引用，可以对堆中的对象进行操作。如：
```
StringBuffer str = new StringBuffer("Helloword");
```
&emsp;变量str指向StringBuffer实例所在的堆空间，通过str可以操作该对象。
![字符串引用在内存中的表现](https://mmbiz.qpic.cn/mmbiz_png/g1iakl3kULHBiaUbd5yMicObIuwhehZibAbsjEFzcSxsIcU9GLhwupPbH9gEQw35CtibUMriaCaJ0Q4wnmOK5gX0F3LA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

&emsp;强引用的特点：
  1. 强引用可以**直接访问目标对象**。

  2. 强引用所指向的对象**在任何时候都不会被系统回收**。JVM宁愿抛出OOM异常，也不会回收强引用所指向的对象。

  3. 强引用**可能导致内存泄漏**。
---
#### 03 软引用

&emsp;软引用是除了强引用外，最强的引用类型。可以通过`java.lang.ref.SoftReference`使用软引用。一个持有软引用的对象，不会被JVM很快回收，JVM会根据当前堆的使用情况来判断何时回收。当堆使用率近阈值时，才回去回收软引用的对象。
因此，软引用可以用于实现对内存敏感的高速缓存。

&emsp;SoftReference的特点是它的一个实例保存一个Java对象的软引用，该软引用的存在不妨碍垃圾收集线程对该Java对象的回收。也就是说，一旦SoftReference保存了对一个Java对象的软引用后，在垃圾线程对这个Java对象回收前，
SoftReference类所提供的 `get()` 方法返回Java对象的强引用。一旦垃圾线程回收该Java对象之后，`get()`方法将返回null。

下面举一个例子说明软引用的使用方法：

&emsp;在你的IDE设置参数 `-Xmx2m -Xms2m` 规定内存大小为2m。
```java
@Test
public void test3(){
  MyObject obj = new MyObject();
  SoftReference sf = new SoftReference<>(obj);
  obj = null;
  //byte[] bytes = new byte[1024*100];
  //System.gc();
  System.out.println("是否被回收" + sf.get());
}
```
&emsp;运行结果：
```java
是否被回收 cn.zyzpp.MyObject@42110406
```
&emsp;打开被注释掉的new byte[1024*100]语句，这条语句请求一块大的堆空间，使堆内存使用紧张。并显示的再调用一次GC，结果如下：
```java
是否被回收 null
```
说明在**系统内存紧张的情况下，软引用被回收。**

---

#### 04 弱引用
&emsp;弱引用是一种比软引用较弱的引用类型。**在系统GC时，只要发现弱引用，不管系统堆空间是否足够，都会将对象进行回收。**
在java中，可以用 `java.lang.ref.WeakReference` 实例来保存对一个Java对象的弱引用。
```java
public void test3(){
  MyObject obj = new MyObject();
  WeakReference wf = new WeakReference(obj);
  obj = null;
  System.out.println("是否被回收" + wk.get());
  System.gc();
  System.out.println("是否被回收" + wk.get());
}
```
运行结果：
```java
是否被回收 cn.zyzzpp.MyObject@42110406
是否被回收 null
```
>软引用、弱引用都非常适合来保存那些可有可无的缓存数据，如果这么做，当系统内存不足时，这些缓存数据会被回收，不会导致内存溢出。
而当内存资源充足时，这些缓存数据又可以存在相当长的时间，从而起到加速系统的作用。

---
##### 05 虚引用
&emsp;虚引用是所有类型中最弱的一个。一个持有虚引用的对象，和没有引用几乎是一样的，随时可能被垃圾回收器回收。
当试图通过虚拟引用的 `get()` 方法取得强引用时，总是会失败。并且，**虚引用必须和引用队列一起使用，它的作用在于跟踪垃圾回收过程。**

&emsp;当垃圾回收器准备回收一个对象时，如果发现它还有虚引用，就会在垃圾回收后，销毁这个对象，将这个虚引用加入引用队列。程序可以通过判断引用队列中是否已经加入了虚引用，来了解被引用对象是否将要被垃圾回收。
如果程序发现某个虚引用已经被加入到引用队列，那么就可以在所引用的对象的内存被回收之前采取必要的行动。

```java
public void test3(){
  MyObject obj = new MyObject();
  ReferenceQueue<Object> referenceQueue = new ReferenceQueue<>();
  PhantomReference pf = new PhantomReference<>(obj, referenceQueue);
  obj = null;
  System.out.println("是否被回收" + pf.get());
  System.gc();
  System.out.println("是否被回收" + pf.get());
}
```
运行结果：
```java
是否被回收 null
是否被回收 null
```
对虚引用的get()操作，总是返回null，因为 `pf.get()` 方法的实现如下：
```java
public T get() {
  return null;
}
```
---
#### 06 弱引用典例

&emsp;WeakHashMap类在 `java.util` 包内，它实现了Map接口，是HashMap的一种实现，它使用弱引用作为内部数据的存储方案。WeakHashMap是弱引用的一种典型应用，它可以作为简单的缓存表解决方案。

&emsp;以下两段代码分别使用 **WeakHashMap** 和 **HashMap** 保存大量的数据：
```java
@Test
public void test4() {
  Map map;
  map = new WeakHashMap<String, Object>();
  for (int i = 0; i < 10000; i++){
    map.put("key" + i, new byte[i]);
  }
  // map = new HashMap<String, Object>();
  // for (int i = 0; i < 10000; i++){
  //    map.put("key" + i, new byte[i]);
  //}
}
```
&emsp;使用 `-Xmx2M` 限定堆内存，使用 `WeakHashMap` 的代码正常运行结束，而使用 `HashMap` 的代码段抛出异常
```java
java.lang.OutOfMemoryError: Java heap space
```

&emsp;由此可见，`WeakHashMap` 会在系统内存紧张时使用弱引用，自动释放掉持有弱引用的内存数据。

&emsp;但如果 `WeakHashMap` 的key都在系统内持有强引用，那么 `WeakHashMap` 就退化为普通的 `HashMap`，因为所有的表项都无法被自动清理。
