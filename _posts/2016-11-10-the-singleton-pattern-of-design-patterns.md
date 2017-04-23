---
layout: post
title:  "(一)设计模式之单例模式"
page.date:   2016-11-10 16:30:28 +0800
categories: jekyll update
---

> 导语： 今天在官网学习网络访问框架**Volley**中， google推荐我们在创建**RequestQueue** 对象时使用*单例模式*，之前也看过一些单例模式讲解的文章，还是不够熟练，在这里总结下，方便自己以后能快速复习；


## 单例模式简介
**单例模式** ，英文名称 Singleton pattern，常用的软件设计模式。
数学与逻辑学中，singleton定义为**“有且仅有一个元素的集合”**。

单例模式最初的定义出现于*《设计模式》*（艾迪生维斯理, 1994）：**“保证一个类仅有一个实例，并提供一个访问它的全局访问点。”**
**Java**中单例模式定义：**“一个类有且仅有一个实例，并且自行实例化向整个系统提供。”**
> 综上所述，Singleton模式主要作用是保证在Java应用程序中，一个类Class只有一个实例存在。

## 一，单例模式的几种常见写法
### 1懒汉式，线程不安全``` （很少使用）```
> 这是最简单的懒汉单例模式，致命的是在多线程不能正常工作。

```
public class Singleton {
    //定义一个变量类存储创建好的实例对象 默认为空
    //因为需要在静态方法中访问 加上static 修饰符
    private static Singleton instance =null;

    //私有化构造方法 防止外部创建对象
    private Singleton() {
    }

    //提供一个类静态方法返回单例对象 可以全局访问
    public static Singleton getInstance(){
        //懒就在这 当第一次有方法访问才创建实例
        //但是之后不会初始化对象
        if (instance==null){
            instance=new Singleton();
        }
        return instance;
    }
}
```

### 2.懒汉式，线程安全``` （很少使用）```
> 使用关键字synchronized同步锁实现同步控制，变成线程安全。这种写法能够在多线程中很好的工作，而且看起来它也具备很好的lazy loading，但是，遗憾的是，效率很低，99%情况下不需要同步。

```
public class Singleton {  
    private static Singleton instance;  
    private Singleton (){}  
    public static synchronized Singleton getInstance() {  
    if (instance == null) {  
        instance = new Singleton();  
    }  
    return instance;  
    }  
} 
```

### 3.饿汉式``` （常用）```
> 这种方式基于java虚拟机类加载机制，static变量只会在类装载的时候初始化一次，并且多个实例共享内存区域，避免了多线程的同步问题，不过，instance在类装载时就实例化，虽然导致类装载的原因有很多种，在单例模式中大多数都是调用getInstance方法， 但是也不能确定有其他的方式（或者其他的静态方法）导致类装载。

```
public class Singleton{
    //在自己内部定义自己的一个实例，只供内部调用
    private static final Singleton instance = new Singleton();
    private Singleton(){
        //do something
    }
    //这里提供了一个供外部访问本class的静态方法，可以直接访问
    public static Singleton getInstance(){
        return instance;
    }
}
```

### 4.双重校验锁 , 线程安全的懒汉式单例模式``` （有需求时使用）```
> 双重检查加锁: 即线程安全又能够性能不受太大的影响。在开源的greenrobot的EventBus中，EventBus类就采用这样的双重检查加锁的单例模式。 
```
public class SynSingleton {
    //对保存的对象 添加volatile关键字
    //volatile 修饰的变量值 不会被本地线程缓存 
    //所有的操作都是直接操作共享内存 保证多个线程能够正确的处理该变量 
    private volatile static SynSingleton instance = null;

    //私有化构造方法 防止外部创建对象
    private SynSingleton() {
    }

    //提供一个类静态方法返回单例对象 可以全局访问
    public static SynSingleton getInstance() {
        //先检查实例是否为空 不为空进入代码块
        if (instance == null) {
            //同步块 线程安全地创建实例
            synchronized (SynSingleton.class) {
                //再次检查实例是否为空 为空才真正的创建实例
                if (instance == null) {
                    instance = new SynSingleton();
                }
            }
        }
        return instance;
    }
}
```

### 5. 静态内部类``` （推荐使用）```
> - 实现的代码思路就是，用静态初始化器的方式，由虚拟机保证线程安全。用类级内部类负责创建实例，只要不使用到这个类级内部类就不会创建实例。两者结合就实现了延迟加载和线程安全。

```
public class HolderSingleton {
    /**
     * 类级内部类 也就是静态的成员式内部类 该内部类的实例与外部类的实例没有依赖
     * 而且只有被调用的时候才会被装载，从而实现延迟加载
     */
    private static class SingletonHolder{
        //静态初始化器 由虚拟机保证线程安全
        private static HolderSingleton INSTANCE= new HolderSingleton();
    }

    private HolderSingleton() {
    }

    public static HolderSingleton getInstance(){
        return SingletonHolder.INSTANCE;
    }
}
```

### 6.枚举
```
public enum  EnumSingleton {
    /**
     * 枚举元素 它就代表单例的一个实例
     */
    uniqueInstance;

    public void setUniqueInstance(){
        //对应普通单例的对象方法或者是功能操作
    }

}
```

---

## 二，单例模式在Volley中的使用
> Volley对于只使用一次的网络请求，常用Volley.newRequestQueue(this)方法来实例化RequestQueue对象。而对于需要频繁使用，贯穿整个生命周期的Request，Google推荐我们使用单例模式来创建一个类包含RequestQueue类对象，以及其他方法。这样我们实例化整个类的时候，既保证了也能保证单例类MySingleton是唯一的，成员变量RequestQueue对象也是唯一的。

> > 这里使用的是第二种“懒汉式，线程安全”的单例模式。

```
public class MySingleton {
    private static MySingleton mInstance;
    private RequestQueue mRequestQueue;
    private static Context mCtx;

    private MySingleton(Context context) {
        mCtx = context;
        mRequestQueue = getRequestQueue();
    }

    public static synchronized MySingleton getInstance(Context context) {
        if (mInstance == null) {
            mInstance = new MySingleton(context);
        }
        return mInstance;
    }

    public RequestQueue getRequestQueue() {
        if (mRequestQueue == null) {
            // getApplicationContext() is key, it keeps you from leaking the
            // Activity or BroadcastReceiver if someone passes one in.
            mRequestQueue = Volley.newRequestQueue(mCtx.getApplicationContext());
        }
        return mRequestQueue;
    }

    public <T> void addToRequestQueue(Request<T> req) {
        getRequestQueue().add(req);
    }
}
```

> 那么如何实例化RequestQunce呢

```
// Get a RequestQueue
RequestQueue queue = MySingleton.getInstance(this.getApplicationContext()).getRequestQueue();
```

---

资料参考：

1.[设计模式-单例模式(Singleton)各种写法和分析比较](http://blog.csdn.net/card361401376/article/details/51340822)

2.[单例模式的七种写法](http://cantellow.iteye.com/blog/838473)

3.[单例模式_百度百科](http://baike.baidu.com/link?url=YwX44KlXoaDymnkfJ7KLOTdNkVDDL4LmU3KHX5jfZfUMAU5Td_2Eq-CWM7IOKW_xzA2ex7lz-G2FhzrlRUVXq_)


