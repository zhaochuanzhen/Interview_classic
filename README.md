# 面试宝典
[HashMap](markdown/HashMap.md)

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

​	`软引用`比`强引用`的级别要低一点，当`JVM`发现堆内存空间不足时，会尝试回收`软引用`的对象，只要当回收完`软引用`的对象空间之后仍然堆内存不足，才会报`OOM`异常。



- 弱引用
- 虚引用
