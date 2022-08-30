# 总结
## 引用

> `JDK1.2` 之后，将引用划分为`强引用`、`软引用`、`弱引用`、`虚引用`

- 强引用

​	`强引用`是最常用的一种引用关系，只要`强引用`关系存在，那么永远不可能被`gc`，当堆内存不足时，`JVM`会报`OOM`异常。

​	如下`referenceTest`指针指向`ReferenceTest`对象，这就构成强引用关系，只要强引用关系存在，那么`System.gc()`是不会回收这块内存，因此前两行打印的对象地址就可以解释。当执行`referenceTest = null`之后，强引用关系断开，此时执行`System.gc()`方法时会回收堆内存、销毁`ReferenceTest`对象，因此执行了`finalize()`方法且打印了`对象销毁`。

```java
public class ReferenceTest {

    public static void main(String[] args) {
        ReferenceTest referenceTest = new ReferenceTest();
        System.out.println(referenceTest);

        System.gc();
        System.out.println(referenceTest);

        referenceTest = null;
        System.out.println(referenceTest);

        System.gc();
        System.out.println(referenceTest);
    }

    @Override
    protected void finalize() throws Throwable {
        super.finalize();
        System.out.println("对象销毁");
    }
}
```

```java
ReferenceTest@74a14482
ReferenceTest@74a14482
null
对象销毁
null
```

- 软引用

​	`软引用`比`强引用`的级别要低一点，当`JVM`发现堆内存空间不足时，会尝试回收`软引用`的对象，只要当回收完`软引用`的对象空间之后仍然堆内存不足，才会报`OOM`异常。软引用可以用作缓存。

​	如下代码所示，首先设置JVM运行参数`-Xms20m -Xmx20m`，然后执行如下代码。首先`JVM`分配`20M`的堆内存空间，先创建`softCacheData`软引用对象，占用`10m`空间，此时调用`softCacheData.get()`是有值的，然后调用`System.gc()`，此时软引用仍然占用空间、未被回收。当再分配`10m`空间时发现堆内存空间不足了，此时`软引用`空间会被释放。

```java
public class ReferenceTest {

    public static void main(String[] args) throws InterruptedException {
        // 10MB的数组
        SoftReference<byte[]> softCacheData = new SoftReference<>(new byte[10 * 1024 * 1024]);

        System.out.println(softCacheData.get());
        //GC回收
        System.gc();
        Thread.sleep(1000);

        System.out.println(softCacheData.get());

        //再创建一个150m的数据
        byte[] newCacheData = new byte[10 * 1024 * 1024];

        System.out.println(softCacheData.get());
    }
    
}
```

```java
[B@74a14482
[B@74a14482
null
```

- 弱引用

​	`弱引用`只要垃圾回收方法一执行，则会释放空间。

```java
public class ReferenceTest {

    public static void main(String[] args) throws InterruptedException {
        WeakReference<ReferenceTest> wr = new WeakReference<>(new ReferenceTest());
        System.out.println(wr.get());

        System.gc();

        System.out.println(wr.get());
    }

    @Override
    protected void finalize() throws Throwable {
        super.finalize();
        System.out.println("对象销毁");
    }
}
```

```java
ReferenceTest@74a14482
null
对象销毁
```



- 虚引用

## ACID靠什么来保证

## [Mysql](markdown/mysql.md)

