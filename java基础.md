### 基本

- **与运算（&）**参加运算的两个数据，按二进制位进行“**与**”运算。

  - 运算规则：0&0=0;  0&1=0;  1&0=0;   1&1=1;
  - **即：两位同时为“1”，结果才为“1”，否则为0**

- 或运算（|）参加运算的两个对象，按二进制位进行“**或**”运算。

  - 运算规则：0|0=0；  0|1=1；  1|0=1；  1|1=1；
  - **即 ：参加运算的两个对象只要有一个为1，其值为1**

- 异或运算（^）  参加运算的两个数据，按二进制位进行“**异或**”运算。

  - 运算规则：0^0=0；  0^1=1；  1^0=1；  1^1=0；
  - **即：参加运算的两个对象，如果两个相应位为“异”（值不同），则该位结果为1，否则为0**

  

### <mark>bio与nio的区别 </mark>

        <mark>**bio同步阻塞io**</mark>: 在此种方式下，⽤户进程在发起一个IO操作以后，必须等待IO操作的完成，只有当真正完成了IO操作以后，用户进程才能运行。JAVA传统的IO模型属于此种⽅式!  
        **<mark>nio同步⾮阻塞式I/O</mark>**：java NIO采用了双向通道进行数据传输，在通道上我们可以注册我们感兴趣的事件:连接事件、读写事件; NIO主要有三⼤核⼼部分:Channel(通道)，Buffer(缓冲区), Selector。传统IO基于字节流和字符流进行操作，而NIO基于Channel和 Buffer(缓冲区)进行操作，数据总是从通道读取到缓冲区中，或者从缓冲区写⼊到通道中。Selector(选择区)⽤用于监听多个通道的事件(⽐如:连接打开，数据到达)。因此，单个线程可以监听多个数据通道。

- BIO (Blocking I/O):同步阻塞I/O模式，数据的读取写入必须阻塞在一个线程内等待其完成。这⾥使用那个经典的烧开⽔例子，这⾥假设⼀个烧开⽔的场景，有一排水壶在烧开水，BIO的工作模式就是， 叫⼀个线程停留在⼀个水壶那，直到这个⽔壶烧开，才去处理下一个水壶。但是实际上线程在等待⽔壶烧开的时间段什么都没有做。  
  
- NIO (New I/O):同时⽀持阻塞与非阻塞模式，但这⾥我们以其同步⾮阻塞I/O模式来说明，那么什么叫做同步⾮阻塞?如果还拿烧开水来说，NIO的做法是叫⼀个线程不断的轮询每个水壶的状态，看看是否有⽔壶的状态发⽣了改变，从而进⾏下一步的操作。

- AIO ( Asynchronous I/O):异步非阻塞I/O模型。异步⾮阻塞与同步非阻塞的区别在哪⾥里?异步⾮阻塞无需一个线程去轮询所有 IO操作的状态改变，在相应的状态改变后，系统会通知对应的线程来处理。对应到烧开水中就是，为每个⽔壶上⾯装了一个开关，⽔烧开之后，⽔壶会自动通知我⽔烧开了。

### <mark>select与poll的区别 </mark>

1. io多路复用:
   
   - **概念**:   IO多路复用是指内核一旦发现进程指定的一个或者多个IO条件准备读取，它就通知该进程
   
   - **优势**:   与多进程和多线程技术相比，I/O多路复用技术的最大优势是系统开销小，系统不必创建进程/线程，也不必维护这些进程/线程，从而⼤大减⼩了系统的开销。
   
   - **系统**:   ⽬前⽀持I/O多路复用的系统调用有 select，pselect，poll，epoll。

2. select:   目前⼏乎在所有的平台上支持，其良好跨平台⽀持也是它的⼀个优点。select的一个缺点在于单个进程能够监视的文件描述符的数量存在最⼤限制，在Linux上⼀般为1024，可以通过修改宏定义甚⾄至重新编译内核的⽅式提升这一限制，但是这样也会造成效率的降低。

3. poll:它没有最⼤大连接数的限制，原因是它是基于链表来存储的，但是同样有⼀一个缺点: 
   
   - ⼤量的fd的数组被整体复制于⽤户态和内核地址空间，⽽不管这样的复制是不是有意义。  
   
   - poll还有⼀个特点是“⽔平触发”，如果报告了fd后，没有被处理，那么下次poll时会再次报告该fd。

> epoll跟select都能提供多路I/O复用的解决方案。在现在的Linux内核里有都能够支持，其中epoll是Linux所特有，而select则应该是POSIX所规定，一般操作系统均有实现。

### <mark>静态代理和动态代理</mark>

#### 静态代理

静态代理，代理类和被代理的类实现了同样的接口，代理类同时持有被代理类的引用，这样，当我们需要调用被代理类的方法时，可以通过调用代理类的方法来做到。

#### 与装饰者模式的区别

实际上，在装饰器模式和代理模式之间还是有很多差别的。装饰器模式关注于在一个对象上动态的添加方法，然而代理模式关注于控制对对象的访问。换句话说，用代理模式，代理类（proxy class）可以对它的客户隐藏一个对象的具体信息。因此，当使用代理模式的时候，我们常常在一个代理类中创建一个对象的实例。并且，当我们使用装饰器模式的时候，我们通常的做法是将原始对象作为一个参数传给装饰者的构造器。

> 我们可以用另外一句话来总结这些差别：使用代理模式，代理和真实对象之间的的关系通常在编译时就已经确定了，而装饰者能够在运行时递归地被构造。

先看看两者的 UML 类图区别： 

##### 代理模式   <img src="data:image/png;base64,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" title="" alt="image" data-align="left">

##### **装饰者模式** <img src="data:image/png;base64,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" title="" alt="image" data-align="left">

两者伪代码：  
代理模式:

```java
Interface Subject {
    void doAction()
}

public class RealSubject implements Subject{
    @Override
    public void doAction() {};
}


public class Proxy implements Subject{
       private RealSubject realSubject;

       public Proxy(RealSubject realSubject) {
               //关系在编译时确定
               //这个地方就是固定了一个对象
            this.realSubject = realSubject;
       }

       @Override
       public void doAction() {
             ….
             realSubject.doAction();
             ….
       }
}
```

// 装饰者模式

```java
Interface Component {
    void doAction()}

public class ConcreteComponent implement Component {
    @Override
    public void doAction() {};
}

public class Decorator implements Component {
       private Component component;

       public Decorator(Component component) {
             //关系在编译时没确定
             // 因为传入的是一个interface 
            this.component = component;
       }
       public void doAction() {
             ….
             component.doAction();
             ….
       }
}
```

其实代理模式和装饰者模式侧重点不一样，代理模式重点在于明确了被代理的类。如上例中，秘书很明确要代理的是的领导。而装饰者模式侧重于拓展类的方法，装饰类持有的实现Component接口的类的对象不是固定的，也就是说，装饰类可以根据在调用时传入的参数，装饰任意一个实现了 Component 接口的类。

###### 动态代理

动态代理的根据实现方式的不同可以分为 JDK 动态代理和 CGlib 动态代理。  

- JDK 动态代理：利用反射机制生成一个实现代理接口的类，在调用具体方法前调用InvokeHandler来处理。  
- CGlib 动态代理：利用ASM（开源的Java字节码编辑库，操作字节码）开源包，将代理对象类的class文件加载进来，通过修改其字节码生成子类来处理。  
- 区别：JDK代理只能对实现接口的类生成代理；CGlib是针对类实现代理，对指定的类生成一个子类，并覆盖其中的方法，这种通过继承类的实现方式，不能代理final修饰的类。

## JDK 动态代理

还是以上面的例子为例：  
首先，定一个类实现 `InvocationHandler` 接口，并实现 invoke 方法：

```java
package com.sharpcj;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;

public class WorkInvocationHandler implements InvocationHandler {
    private Object object;

    public WorkInvocationHandler(Object object) {
        this.object = object;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("object: " + object.getClass().getSimpleName());
        System.out.println("proxy: " + proxy.getClass().getSimpleName());

        if ("meeting".equals(method.getName())) {
            System.out.println("代理先准备会议材料...");
            return method.invoke(object, args);
        } else if ("evaluate".equals(method.getName())) {
            if(args[0] instanceof String) {
                if ("James".equals(args[0])) {
                    System.out.println("James 犯过错误，所以考评分数较低...");
                    return 70;
                }
            }
            return method.invoke(object, args);
        }
        return null;
    }
}
```

然后通过 `Proxy.newProxyInstance()` 方法创建代理对象：

```java
package com.sharpcj;

import java.lang.reflect.Proxy;

public class TestApp {
    public static void main(String[] args) {
        /*Leader leader = new Leader();        Secretary secretary = new Secretary(leader);        secretary.meeting();        secretary.evaluate("Joy");*/

        Leader leader = new Leader();
        IWork proxy = (IWork) Proxy.newProxyInstance(Leader.class.getClassLoader(),
                new Class[]{IWork.class}, new WorkInvocationHandler(leader));
        proxy.meeting();
        proxy.evaluate("Joy");
        proxy.evaluate("James");
    }
}
```

我们看到，通过 WorkInvocationHandler 类，我们同样可以代理 Leader 类的方法的实现，实际上我们实现的是任意的方法的实现，只是我们在创建代理对象的时候传入的是 Iwork 接口以及 Leader 类对象。  
这里需要注意的是：在 InvocationHandler 接口的 invoke 方法中第一个参数 proxy, 并不是我们调用方法的对象，那这个参数是什么呢？代码中，我特别增加相应打印，打印出了 proxy 的类名，实际上 proxy 是代理对象本身，它的意义在于，我们可以在 invoke 方法中，返回该代理对象，然后进行连续调用。  
看如下例子：

```java
package com.sharpcj.proxytest;

public interface IWork {
    IWork work(String subject);
}
```

```java
package com.sharpcj.proxytest;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;

public class WorkInvocationHandler implements InvocationHandler {
    private Object object;

    public WorkInvocationHandler(Object object) {
        this.object = object;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        if ("work".equals(method.getName())){
            System.out.println("--- work: " + args[0]);
            return proxy;
        }
        return null;
    }
}
```

```java
package com.sharpcj.proxytest;

import java.lang.reflect.Proxy;

public class TestApp {
    public static void main(String[] args) {
        IWork worker = (IWork) Proxy.newProxyInstance(IWork.class.getClassLoader(), new Class[]{IWork.class},
                new WorkInvocationHandler(new IWork() {
                    @Override
                    public IWork work(String subject) {
                        return null;
                    }
                }));
        worker.work("AAA").work("BBB").work("CCC");
    }
}
```

## CGlib 动态代理实现

首先添加 cglib 依赖  
build.gradle 文件：

```js
... 

dependencies {
    // 引入 cglib 库
    compile 'cglib:cglib:3.1'
    testCompile group: 'junit', name: 'junit', version: '4.12'
}
...
```

前面说了，cglib 针对类进行代理，我们以上面的 Leader 类为例，先创建一个类实现 `MethodInterceptor`接口：

```java
package com.sharpcj;

import net.sf.cglib.proxy.MethodInterceptor;
import net.sf.cglib.proxy.MethodProxy;

import java.lang.reflect.Method;

public class LeaderMethodInterceptor implements MethodInterceptor {
    @Override
    public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
        if ("meeting".equals(method.getName())) {
            System.out.println("代理先准备会议材料...");
            return methodProxy.invokeSuper(o, objects);
        } else if ("evaluate".equals(method.getName())) {
            if(objects[0] instanceof String) {
                if ("James".equals(objects[0])) {
                    System.out.println("James 犯过错误，所以考评分数较低...");
                    return 70;
                }
            }
            return methodProxy.invokeSuper(o, objects);
        }
        return null;
    }
}
```

测试代码：

```java
package com.sharpcj;

import net.sf.cglib.core.DebuggingClassWriter;
import net.sf.cglib.proxy.Enhancer;

import java.lang.reflect.Proxy;

public class TestApp {
    public static void main(String[] args) {
        // System.setProperty(DebuggingClassWriter.DEBUG_LOCATION_PROPERTY, "D:\\temp\\code");  //保存生成的 class 文件
        Enhancer enhancer = new Enhancer(); // 通过CGLIB动态代理获取代理对象的过程
        enhancer.setSuperclass(Leader.class); // 设置enhancer对象的父类
        enhancer.setCallback(new LeaderMethodInterceptor()); // 设置enhancer的回调对象
        Leader proxy= (Leader)enhancer.create(); // 创建代理对象

        // 通过代理对象调用目标方法
        proxy.meeting();
        proxy.evaluate("Joy");
        proxy.evaluate("James");
    }
}
```

`MethodInterceptor` 接口只有一个 `intercept` 方法，这个方法有4个参数：  
1）obj表示增强的对象，即实现这个接口类的一个对象；  
2）method表示要被拦截的方法；  
3）args表示要被拦截方法的参数；  
4）proxy表示要触发父类的方法对象；

需要注意的是，实际调用是 `methodProxy.invokeSuper()`, 如果使用 `invoke()` 方法，则需要传入被代理的类对象，否则出现死循环，造成 stackOverflow 。

**<u><mark>Cglib和jdk动态代理的区别？</mark></u>**

- Jdk动态代理：利用拦截器（必须实现InvocationHandler）加上反射机制生成一个代理接口的匿名类，在调用具体方法前调用InvokeHandler来处理

- Cglib动态代理：利用ASM框架，对代理对象类生成的class文件加载进来，通过修改其字节码生成子类来处理

- <mark>动态代理性能</mark>，在1.6和1.7的时候，JDK动态代理的速度要比CGLib动态代理的速度要慢，但是并没有教科书上的10倍差距，在JDK1.8的时候，JDK动态代理的速度已经比CGLib动态代理的速度快很多了，但是JDK动态代理和CGLIB动态代理的适用场景还是不一样的哈！

**<mark>Spring如何选择是用JDK还是cglib？</mark>**

- 当bean实现接口时，会用JDK代理模式

- 当bean没有实现接口，用cglib实现

- 可以强制使用cglib（在spring配置中加入<aop:aspectj-autoproxy proxyt-target-class=”true”/>）

### <mark> HashMap 解析</mark>

1.7版本

扩容使用了头插法，高并发的情况下会导致环形链表的问题

```java
public V put(K key, V value) { //put操作
        if (table == EMPTY_TABLE) { 
            //如果table数组是一个空的数组，需要执行inflateTable构造数组
            inflateTable(threshold);
        }
        if (key == null)
            //如果key为null 则 调用putForNullKey方法
            //和hashTable（hashTable不允许 空值key和空值value 存在的）不一样
            return putForNullKey(value);
        int hash = hash(key);
        //获取key的hash值
        int i = indexFor(hash, table.length);
        //这里判断如果key是否在对应的数组 
        for (Entry<K,V> e = table[i]; e != null; e = e.next) {
            Object k;
            if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {
                V oldValue = e.value;
                e.value = value;
                e.recordAccess(this);
                return oldValue;
            }
        }
        modCount++;
        addEntry(hash, key, value, i);
        return null;
    }


     static int indexFor(int h, int length) {
        // assert Integer.bitCount(length) == 1 : 
        //"length must be a non-zero power of 2";
        // 这里为什么说length 一定是2的n次方 因为在 与操作中只有 1和1 相同才能为1
        // 否则为0，只有在2的n次方下在减一 才能保证在与操作中的一方都是1
        // 才能让hash值离散的更均匀，不然不过与操作 length过多0的话，基本上在
        //数组那几个下标下，导致链表越来越长，查询的效率差
        return h & (length-1);
    }

    void addEntry(int hash, K key, V value, int bucketIndex) {
        if ((size >= threshold) && (null != table[bucketIndex])) {
            resize(2 * table.length);
            hash = (null != key) ? hash(key) : 0;
            bucketIndex = indexFor(hash, table.length);
        }

        createEntry(hash, key, value, bucketIndex);
    }


    void createEntry(int hash, K key, V value, int bucketIndex) {
        Entry<K,V> e = table[bucketIndex];
        table[bucketIndex] = new Entry<>(hash, key, value, e);
        size++;
    }

    //扩容的方法
    void resize(int newCapacity) {
        Entry[] oldTable = table;
        int oldCapacity = oldTable.length;
        //如果数组是最大的数值，2的30次方则不在扩容
        if (oldCapacity == MAXIMUM_CAPACITY) {
            threshold = Integer.MAX_VALUE;
            return;
        }
        Entry[] newTable = new Entry[newCapacity];
        //数据转移
        transfer(newTable, initHashSeedAsNeeded(newCapacity));
        table = newTable;
        threshold = (int)Math.min(newCapacity * loadFactor, MAXIMUM_CAPACITY + 1);
    }

    //数据转移
    void transfer(Entry[] newTable, boolean rehash) {
        //使用头插法，从链表第一个Entry 插入到新的table上面
        //会导致 链表的死循环 ，高并发下不推荐使用  
      	//这个是因为头插法导致了 链表的顺序逆反，（线程一 执行完当前链表的逆反，线程二持有的next 不是指向了原本的而是指向了上一个，所以导致第二次while循环的时候拿到的next 是原本一开始的e，这个时候形成了环
        int newCapacity = newTable.length;
        for (Entry<K,V> e : table) {
            while(null != e) {
                Entry<K,V> next = e.next;
                if (rehash) {
                    e.hash = null == e.key ? 0 : hash(e.key);
                }
                int i = indexFor(e.hash, newCapacity);
                e.next = newTable[i];
                newTable[i] = e;
                e = next;
            }
        }
    }

    //只要容量小于整数最大值，switching 就一直为 false，
/*并且 switching 一直为 false，那么 hashSeed 就一直为0。
（JDK7只需要知道这点就够了）有啥用? 
因为下面的transfer()要用到，如果initHashSeedAsNeeded返回的是false，
那么transfer()方法中就不会触发 rehash（也就是重新计算哈希值）。
如果容量大于等于整数最大值，那么 useAltHashing 就为 true，
那么 switching 就为 true，hashSeed 也会被设置成一个随机值
（sun.misc.Hashing.randomHashSeed()，JDK8 没有这个函数了），
想了下，其实就类似于一个偏移值，hashCode值的范围在 -2^(31) ~ 2^(31) - 1 之间，
而允许的最大容量又是 1 << 30, 即2^(30)，
超过整数最大值的话原先的hashCode值的范围也得变大，我的理解就这样吧。
因为JDK8不用这个方法，所以不深究。*/
    final boolean initHashSeedAsNeeded(int capacity) {
        boolean currentAltHashing = hashSeed != 0;
        boolean useAltHashing = sun.misc.VM.isBooted() &&
                (capacity >= Holder.ALTERNATIVE_HASHING_THRESHOLD);
        boolean switching = currentAltHashing ^ useAltHashing;
        if (switching) {
            hashSeed = useAltHashing
                ? sun.misc.Hashing.randomHashSeed(this)
                : 0;
        }
        return switching;
    }
```

1.8

采用了尾插法，

```java
 //1.8的hashMap的put操作
 final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
        Node<K,V>[] tab; Node<K,V> p; int n, i;
         //区别于1.7的hashMap 在如果table为空的情况下直接调用resize方法
         //在resize方法里面直接做新增数组操作，去掉了inflateTable方法
        if ((tab = table) == null || (n = tab.length) == 0)
            n = (tab = resize()).length;
        //不同于1.7版本 addEntry 换成了newNode
        if ((p = tab[i = (n - 1) & hash]) == null)
            tab[i] = newNode(hash, key, value, null);
        else {
            Node<K,V> e; K k;
            if (p.hash == hash &&
                ((k = p.key) == key || (key != null && key.equals(k))))
                e = p;
            else if (p instanceof TreeNode)
                e = ((TreeNode<K,V>)p).putTreeVal(this, tab, hash, key, value);
            else {
                for (int binCount = 0; ; ++binCount) {
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);
 
             //static final int TREEIFY_THRESHOLD = 8; 链表转红黑树
            //static final int UNTREEIFY_THRESHOLD = 6; 红黑树转链表
                //TREEIFY_THRESHOLD-1  是因为binCount 从0开始
                        if (binCount >= TREEIFY_THRESHOLD - 1) 
                            treeifyBin(tab, hash);
                        break;
                    }
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
            }
            if (e != null) { // existing mapping for key
                V oldValue = e.value;
                if (!onlyIfAbsent || oldValue == null)
                    e.value = value;
                afterNodeAccess(e);
                return oldValue;
            }
        }
        ++modCount;
        if (++size > threshold)
            resize();
        afterNodeInsertion(evict);
        return null;
    }
 
     
      final Node<K,V>[] resize() {
        Node<K,V>[] oldTab = table;
        int oldCap = (oldTab == null) ? 0 : oldTab.length;
        int oldThr = threshold;
        int newCap, newThr = 0;
        if (oldCap > 0) {
            if (oldCap >= MAXIMUM_CAPACITY) {
                threshold = Integer.MAX_VALUE;
                return oldTab;
            }
            else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                     oldCap >= DEFAULT_INITIAL_CAPACITY)
                newThr = oldThr << 1; // double threshold
        }
        else if (oldThr > 0) // initial capacity was placed in threshold
            newCap = oldThr;
        else {               // zero initial threshold signifies using defaults
            newCap = DEFAULT_INITIAL_CAPACITY;
            newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
        }
        if (newThr == 0) {
            float ft = (float)newCap * loadFactor;
            newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                      (int)ft : Integer.MAX_VALUE);
        }
        threshold = newThr;
        @SuppressWarnings({"rawtypes","unchecked"})
        Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
        table = newTab;
        if (oldTab != null) {
            for (int j = 0; j < oldCap; ++j) {
                Node<K,V> e;
                if ((e = oldTab[j]) != null) {
                    oldTab[j] = null;
                    if (e.next == null)
                        newTab[e.hash & (newCap - 1)] = e;
                    else if (e instanceof TreeNode)
                        ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                    else { // preserve order
                        Node<K,V> loHead = null, loTail = null;
                        Node<K,V> hiHead = null, hiTail = null;
                        Node<K,V> next;
                        do {
                            next = e.next;
                            if ((e.hash & oldCap) == 0) {
                                if (loTail == null)
                                    loHead = e;
                                else
                                    loTail.next = e;
                                loTail = e;
                            }
                            else {
                                if (hiTail == null)
                                    hiHead = e;
                                else
                                    hiTail.next = e;
                                hiTail = e;
                            }
                        } while ((e = next) != null);
                        if (loTail != null) {
                            loTail.next = null;
                            newTab[j] = loHead;
                        }
                        if (hiTail != null) {
                            hiTail.next = null;
                            newTab[j + oldCap] = hiHead;
                        }
                    }
                }
            }
        }
        return newTab;
    }
```

### Hashmap的结构，1.7和1.8有哪些区别

- 底层数据结构不一样，1.7是**数组+链表**，1.8则是**数组+链表+红黑树结构（当链表长度大于8，转为红黑树）。**
- JDK1.8中resize()方法在表为空时，创建表；在表不为空时，扩容；而JDK1.7中resize()方法负责扩容，inflateTable()负责创建表。
- 1.8中没有区分键为null的情况，而1.7版本中对于键为null的情况调用putForNullKey()方法。但是两个版本中如果键为null，那么调用hash()方法得到的都将是0，**所以键为null的元素都始终位于哈希表table【0】中。**
- 当1.8中的桶中元素处于链表的情况，遍历的同时最后如果没有匹配的，直接将节点添加到链表尾部；而1.7在遍历的同时没有添加数据，而是另外调用了addEntry()方法，将节点添加到链表头部。
- 1.7中新增节点采用头插法，1.8中新增节点采用尾插法。这也是为什么1.8不容易出现环型链表的原因。
- 1.7中是通过更改hashSeed值修改节点的hash值从而达到rehash时的链表分散，而1.8中键的hash值不会改变，rehash时根据（hash&oldCap）==0将链表分散。
- 1.8rehash时保证原链表的顺序，而1.7中rehash时有可能改变链表的顺序（头插法导致）。
- **在扩容的时候：1.7在插入数据之前扩容，而1.8插入数据成功之后扩容。**



### TreeMap内部实现

```java
 static final class Entry<K,V> implements Map.Entry<K,V> {
        K key;
        V value;
        Entry<K,V> left;
        Entry<K,V> right;
        Entry<K,V> parent;
        boolean color = BLACK;
   //部分代码。TreeMap 底层使用的红黑树。color
 }


```



### HashSet内部实现

```java
    public HashSet(int initialCapacity, float loadFactor) {
        map = new HashMap<>(initialCapacity, loadFactor);
    }

    public HashSet(int initialCapacity) {
        map = new HashMap<>(initialCapacity);
    }
    
    HashSet(int initialCapacity, float loadFactor, boolean dummy) {
        map = new LinkedHashMap<>(initialCapacity, loadFactor);
    }

		private static final Object PRESENT = new Object();

		public boolean add(E e) {
        return map.put(e, PRESENT)==null;
    }

    public boolean remove(Object o) {
        return map.remove(o)==PRESENT;
    }
```

### TreeSet 内部实现

```java
    public TreeSet() {
        this(new TreeMap<E,Object>());
    }

    public TreeSet(Comparator<? super E> comparator) {
        this(new TreeMap<>(comparator));
    }

    public boolean add(E e) {
        return m.put(e, PRESENT)==null;
    }

    private static final Object PRESENT = new Object();

    public boolean remove(Object o) {
        return m.remove(o)==PRESENT;
    }
```



### 4个引用

- 强引用    当我们使用new 这个关键字创建对象时被创建的对象就是强引用，如Object object = new Object() 这个Object()就是一个强引用了，如果一个对象具有强引用。==垃圾回收器就不会去回收有强引用的对象。如当jvm内存不足时，具备强引用的对象，虚拟机宁可会报内存空间不足的异常来终止程序，也不会靠垃圾回收器去回收该对象来解决内存==。
- 软引用
  如果一个对象具备软引用，==如果内存空间足够，那么垃圾回收器就不会回收它，如果内存空间不足了，就会回收该对象。当然没有被回收之前，该对象依然可以被程序调用==。
- 弱引用
  如果一个对象只具有弱引用，==只要垃圾回收器在自己的内存空间中线程检测到了，就会立即被回收，对应内存也会被释放掉==。相比软引用弱引用的生命周期要比软引用短很多。不过，如果垃圾回收器是一个优先级很低的线程，也不一定会很快就会释放掉软引用的内存。
- 虚引用
  如果一个对象只具有虚引用，那么它就和没有任何引用一样，随时会被jvm当作垃圾进行回收。



### <mark>内存泄露</mark>

内存泄漏也称作"存储渗漏"，用动态存储分配函数动态开辟的空间，在使用完毕后未释放，结果导致一直占据该内存单元。直到程序结束。通俗来讲就是一个长生命的对象持有了一个短生命对象，就算短生命对象无用，由于长生命对象持有，导致了gc不可回收该短生命对象，使得那块内存被占用。

### <mark>threadlocal会不会出现内存泄漏</mark>

```java
    //thread 类里面持有threadlocalMap 
    ThreadLocal.ThreadLocalMap threadLocals = null;

    /*
     * InheritableThreadLocal values pertaining to this thread. This map is
     * maintained by the InheritableThreadLocal class.
     */
    ThreadLocal.ThreadLocalMap inheritableThreadLocals = null;

     //threadlocal 类里面有内部静态类threadLocalMap，有内部静态Entry，
     //继承了弱引用
     static class ThreadLocalMap {

        /**
         * The entries in this hash map extend WeakReference, using
         * its main ref field as the key (which is always a
         * ThreadLocal object).  Note that null keys (i.e. entry.get()
         * == null) mean that the key is no longer referenced, so the
         * entry can be expunged from table.  Such entries are referred to
         * as "stale entries" in the code that follows.
         */
        static class Entry extends WeakReference<ThreadLocal<?>> {
            /** The value associated with this ThreadLocal. */
            Object value;

            Entry(ThreadLocal<?> k, Object v) {
                super(k);
                value = v;
            }
        }
```

 Thread中维护了ThreadLocalMap，所以ThreadLocalMap的生命周期和Thread（当前线程）一样长。使用不当就可能会导致内存泄漏问题。

![image](data:image/png;base64,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)

TreadLocal对象使用完成或者内存不足时，**ThreadLocalMap的Entry的key(这里因为key是一个弱应用对象)会GC，但是ThreadLocalMap 的value强引用还在，在GC Root（栈帧本地变量表）的时候，不会GC**。如果线程一直运行，当一直使用ThreadLocal的时候，由于Entry的key是弱引用，value是强引用。如果value一直使用，那么引用一直存在。就会出现一个null为key的value的map；仅当线程对象GC的时候ThreadLocalMap 才会GC回收，如果线程长期运行，就会一直出现多个key为null的Entry。这些不会再使用key为null的Entry就存在内存泄漏的风险。ThreadLocal使用完毕，手工调用ThreadLocal对象的remove()方法；当然set与get方法都会检查当前线程的threadlocal副本是否为null。**只是如果我们长期没用这个thread local对象，就需要手动remove，回收内存**。

### threadLocal详解

```java
//threadLocal get方法获取
public T get() {
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null) {
            ThreadLocalMap.Entry e = map.getEntry(this);
            if (e != null) {
                @SuppressWarnings("unchecked")
                T result = (T)e.value;
                return result;
            }
        }
        return setInitialValue();
    }

//threadLocal set方法
public void set(T value) {
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null)
            map.set(this, value);
        else
            createMap(t, value);
 }

/*如何拿到其他线程的ThreadLocal 里面的数据，需要拿到其他thread对象获取threadLocalMap */
ThreadLocalMap getMap(Thread t) {
        return t.threadLocals;
}

/*其实每个thread 维护了一份ThreadLocalMap  */
void createMap(Thread t, T firstValue) {
  t.threadLocals = new ThreadLocalMap(this, firstValue);
}




```



### ThreadGroup详解

Java中使用ThreadGroup类来代表线程组，表示一组线程的集合，可以对一批线程和线程组进行管理。可以把线程归属到某一个线程组中，线程组中可以有线程对象，也可以有线程组，组中还可以有线程，这样的组织结构有点类似于树的形式，如图所示。

![](data:image/jpeg;base64,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)

- 安全

同一个线程组的线程是可以相互修改对方的数据的。但如果在不同的线程组中，那么就不能“跨线程组”修改数据，可以从一定程度上保证数据安全。

- 批量管理

可以批量管理线程或线程组对象，有效地对线程或线程组对象进行组织或控制。

- 线程关联线程组  :一级关联，所谓一级关联就是父对象中有子对象，但并不创建孙对象。比如创建一个线程组，然后将创建的线程归属到该组中，从而对这些线程进行有效的管理。代码示例如下：

```java
public class ThreadGroupTest {
 public static void main(String[] args) {
     ThreadGroup rootThreadGroup = new ThreadGroup("root线程组");
     Thread thread0 = new Thread(rootThreadGroup, new MRunnable(), "线程A");
     Thread thread1 = new Thread(rootThreadGroup, new MRunnable(), "线程B");
     thread0.start();
     thread1.start();
 		}
}
class MRunnable implements Runnable {
     @Override
     public void run() {
         while (!Thread.currentThread().isInterrupted()) {
             System.out.println("线程名: " + Thread.currentThread().getName() + ", 所在线程组: " + 
                                Thread.currentThread().getThreadGroup().getName()) ;
             try {
             			Thread.sleep(1000);
             } catch (InterruptedException e) {
            			e.printStackTrace();
             }
         }
     }
}

/* 
线程名: 线程A, 所在线程组: root线程组
线程名: 线程B, 所在线程组: root线程组
*/
```

- 线程关联线程组：多级关联

所谓的多级关联就是父对象中有子对象，子对象中再创建孙对象也就出现了子孙的效果了。比如使用下图第二个构造方法，将子线程组归属到某个线程组，再将创建的线程归属到子线程组，这样就会有线程树的效果了。

```java
public class ThreadGroupTest {
 public static void main(String[] args) {
       ThreadGroup rootThreadGroup = new ThreadGroup("root线程组");
       Thread thread0 = new Thread(rootThreadGroup, new MRunnable(), "线程A");
       Thread thread1 = new Thread(rootThreadGroup, new MRunnable(), "线程B");
       thread0.start();
       thread1.start();
       ThreadGroup threadGroup1 = new ThreadGroup(rootThreadGroup, "子线程组");
       Thread thread2 = new Thread(threadGroup1, new MRunnable(), "线程C");
       Thread thread3 = new Thread(threadGroup1, new MRunnable(), "线程D");
       thread2.start();
       thread3.start();
 }
}
class MRunnable implements Runnable {
       @Override
       public void run() {
             while (!Thread.currentThread().isInterrupted()) {
                   System.out.println("线程名: " + Thread.currentThread().getName()
                   + ", 所在线程组: " + Thread.currentThread().getThreadGroup().getName()
                   + ", 父线程组: " + Thread.currentThread().getThreadGroup().getParent().getName());
                   try {
                   			Thread.sleep(1000);
                   } catch (InterruptedException e) {
                   			e.printStackTrace();
                   }
             }
       }
}

/*
线程名: 线程A, 所在线程组: root线程组, 父线程组: main
线程名: 线程B, 所在线程组: root线程组, 父线程组: main
线程名: 线程C, 所在线程组: 子线程组, 父线程组: root线程组
线程名: 线程D, 所在线程组: 子线程组, 父线程组: root线程组
*/
```

批量管理组内线程

使用线程组自然是要对线程进行批量管理，比如可以批量中断组内线程，代码示例如下：

```java
public class ThreadGroupTest {
     public static void main(String[] args) {
           ThreadGroup rootThreadGroup = new ThreadGroup("root线程组");
           Thread thread0 = new Thread(rootThreadGroup, new MRunnable(), "线程A");
           Thread thread1 = new Thread(rootThreadGroup, new MRunnable(), "线程B");
           thread0.start();
           thread1.start();
           ThreadGroup threadGroup1 = new ThreadGroup(rootThreadGroup, "子线程组");
           Thread thread2 = new Thread(threadGroup1, new MRunnable(), "线程C");
           Thread thread3 = new Thread(threadGroup1, new MRunnable(), "线程D");
           thread2.start();
           thread3.start();
           rootThreadGroup.interrupt();
           System.out.println("批量中断组内线程");
     }
}
class MRunnable implements Runnable {
     @Override
     public void run() {
           while (!Thread.currentThread().isInterrupted()) {
                 System.out.println("线程名: " + Thread.currentThread().getName()
                                    + ", 所在线程组: " + Thread.currentThread().getThreadGroup().getName()
                                    + ", 父线程组: " + Thread.currentThread().getThreadGroup().getParent().getName());
                 try {
                   Thread.sleep(1000);
                 } catch (InterruptedException e) {
                   e.printStackTrace();
                   break;
                 }
           }
           System.out.println(Thread.currentThread().getName() + "执行结束");
     }
}

/*
线程名: 线程A, 所在线程组: root线程组, 父线程组: main
线程名: 线程B, 所在线程组: root线程组, 父线程组: main
线程名: 线程C, 所在线程组: 子线程组, 父线程组: root线程组
线程名: 线程D, 所在线程组: 子线程组, 父线程组: root线程组
批量中断组内线程
线程A执行结束
线程B执行结束
线程C执行结束
线程D执行结束
*/
```



### <mark>volatile</mark>

###### volatile可以用于在变量前面进行修饰，而被它修饰了的变量有两个作用：

- 保证该变量对所有线程的可见性，所以使用该变量时的值是一定是最新的

- 禁止指令重排序（指令重排序是为了执行效率更快）

volatile的底层实现原理，其底层的实现依然使用到了汇编层面的lock指令，通过总线锁和缓存一致性协议来保证变量的最新值。通过在读写操作前后加入内存屏障来实现禁止指令的重排序，而内存屏障依然是使用lock前缀来实现的。

### 使用volatile来保证禁止指令重排序的场景，单例模式

DCL（double check lock）   双重检查锁定 

==一定要volatile 因为如果出现指令重排，会返回一个半初始化的对象，会出现多线程安全==

```java
public class Student {
  //一定要volatile 因为如果出现指令重排，会返回一个半初始化的对象，会出现多线程安全
    private static volatile Student instance;
    public static Student getInstance(){
//第一次检测instance是否为null, 是为了去创建对象，如果不为空，直接返回，不需要syc的阻塞等待
        if (instance==null){
            synchronized (Student.class){
//第二次检查instance是为了控制，第一次判空的时候有两个线程进来，第一个先执行syc里面的，第二个线程阻塞，然后去执行，为了这个时候不创建两个对象
                if (instance == null){
                    instance = new Student();
                }
            }
        }
        return instance;
    }
}
```

volatile关键字的来禁止重排序，首先我们要知道instance = new Student()分为几个步骤：
　1. 内存分配
　2. 初始化
　3. 返回对象引用
正常情况下是没有问题的，如果第一个线程发生了指令重排序，返回对象引用要先于初始化执行，而其他线程刚好正在进行第一个判断的时候认为instance不为空，直接调用这个未被初始化的instance了，就会出现问题。



### 布隆过滤器

布隆过滤器其实就是加快判定一个元素是否在集合中出现的方法。比如说在一个大字典中，要查找某个单词是否存在，于是我们就可以使用布隆过滤器，快速高效省时省力。

![](data:image/png;base64,/9j/4AAQSkZJRgABAQAASABIAAD/4QBARXhpZgAATU0AKgAAAAgAAYdpAAQAAAABAAAAGgAAAAAAAqACAAQAAAABAAADuKADAAQAAAABAAABWAAAAAD/7QA4UGhvdG9zaG9wIDMuMAA4QklNBAQAAAAAAAA4QklNBCUAAAAAABDUHYzZjwCyBOmACZjs+EJ+/+ICNElDQ19QUk9GSUxFAAEBAAACJGFwcGwEAAAAbW50clJHQiBYWVogB98ACgAOAA0ACAA5YWNzcEFQUEwAAAAAQVBQTAAAAAAAAAAAAAAAAAAAAAAAAPbWAAEAAAAA0y1hcHBs5bsOmGe9Rs1LvkRuvRt1mAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAKZGVzYwAAAPwAAABlY3BydAAAAWQAAAAjd3RwdAAAAYgAAAAUclhZWgAAAZwAAAAUZ1hZWgAAAbAAAAAUYlhZWgAAAcQAAAAUclRSQwAAAdgAAAAgY2hhZAAAAfgAAAAsYlRSQwAAAdgAAAAgZ1RSQwAAAdgAAAAgZGVzYwAAAAAAAAALRGlzcGxheSBQMwAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAB0ZXh0AAAAAENvcHlyaWdodCBBcHBsZSBJbmMuLCAyMDE1AABYWVogAAAAAAAA81EAAQAAAAEWzFhZWiAAAAAAAACD3wAAPb////+7WFlaIAAAAAAAAEq/AACxNwAACrlYWVogAAAAAAAAKDgAABELAADIuXBhcmEAAAAAAAMAAAACZmYAAPKwAAANUAAAE7YAAAn8c2YzMgAAAAAAAQxCAAAF3v//8yYAAAeTAAD9kP//+6L///2jAAAD3AAAwG7/wAARCAFYA7gDASIAAhEBAxEB/8QAHwAAAQUBAQEBAQEAAAAAAAAAAAECAwQFBgcICQoL/8QAtRAAAgEDAwIEAwUFBAQAAAF9AQIDAAQRBRIhMUEGE1FhByJxFDKBkaEII0KxwRVS0fAkM2JyggkKFhcYGRolJicoKSo0NTY3ODk6Q0RFRkdISUpTVFVWV1hZWmNkZWZnaGlqc3R1dnd4eXqDhIWGh4iJipKTlJWWl5iZmqKjpKWmp6ipqrKztLW2t7i5usLDxMXGx8jJytLT1NXW19jZ2uHi4+Tl5ufo6erx8vP09fb3+Pn6/8QAHwEAAwEBAQEBAQEBAQAAAAAAAAECAwQFBgcICQoL/8QAtREAAgECBAQDBAcFBAQAAQJ3AAECAxEEBSExBhJBUQdhcRMiMoEIFEKRobHBCSMzUvAVYnLRChYkNOEl8RcYGRomJygpKjU2Nzg5OkNERUZHSElKU1RVVldYWVpjZGVmZ2hpanN0dXZ3eHl6goOEhYaHiImKkpOUlZaXmJmaoqOkpaanqKmqsrO0tba3uLm6wsPExcbHyMnK0tPU1dbX2Nna4uPk5ebn6Onq8vP09fb3+Pn6/9sAQwACAgICAgIDAgIDBQMDAwUGBQUFBQYIBgYGBgYICggICAgICAoKCgoKCgoKDAwMDAwMDg4ODg4PDw8PDw8PDw8P/9sAQwECAgIEBAQHBAQHEAsJCxAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQEBAQ/90ABAA8/9oADAMBAAIRAxEAPwD90j4auSc/2vc/9+7f/wCNUf8ACNXP/QXuf+/dv/8AGq6qii4XOV/4Rq5/6C9z/wB+7f8A+NUf8I1c/wDQXuf+/dv/APGq6qincdzlf+Eauf8AoL3P/fu3/wDjVH/CNXP/AEF7n/v3b/8AxquqoouFzlf+Eauf+gvc/wDfu3/+NUf8I1c/9Be5/wC/dv8A/Gq6qii4XOV/4Rq5/wCgvc/9+7f/AONUf8I1c/8AQXuf+/dv/wDGq6qii4XOV/4Rq5/6C9z/AN+7f/41R/wjVz/0F7n/AL92/wD8arqqKLhc5X/hGrn/AKC9z/37t/8A41R/wjVz/wBBe5/792//AMarqqKLhc5X/hGrn/oL3P8A37t//jVH/CNXP/QXuf8Av3b/APxquqoouFzlf+Eauf8AoL3P/fu3/wDjVH/CNXP/AEF7n/v3b/8AxquqoouFzlf+Eauf+gvc/wDfu3/+NUf8I1c/9Be5/wC/dv8A/Gq6qii4XOV/4Rq5/wCgvc/9+7f/AONUf8I1c/8AQXuf+/dv/wDGq6qii4XOV/4Rq5/6C9z/AN+7f/41R/wjVz/0F7n/AL92/wD8arqqKLhc5X/hGrn/AKC9z/37t/8A41R/wjVz/wBBe5/792//AMarqqKLhc5X/hGrn/oL3P8A37t//jVH/CNXP/QXuf8Av3b/APxquqoouFzlf+Eauf8AoL3P/fu3/wDjVH/CNXP/AEF7n/v3b/8AxquqoouFzlf+Eauf+gvc/wDfu3/+NUf8I1c/9Be5/wC/dv8A/Gq6qii4XOV/4Rq5/wCgvc/9+7f/AONUf8I1c/8AQXuf+/dv/wDGq6qii4XOV/4Rq5/6C9z/AN+7f/41R/wjVz/0F7n/AL92/wD8arqqKLhc5X/hGrn/AKC9z/37t/8A41R/wjVz/wBBe5/792//AMarqqKLhc5X/hGrn/oL3P8A37t//jVH/CNXP/QXuf8Av3b/APxquqoouFzlf+Eauf8AoL3P/fu3/wDjVH/CNXP/AEF7n/v3b/8AxquqoouFzlf+Eauf+gvc/wDfu3/+NUf8I1c/9Be5/wC/dv8A/Gq6qii4XOV/4Rq5/wCgvc/9+7f/AONUf8I1c/8AQXuf+/dv/wDGq6qii4XOV/4Rq5/6C9z/AN+7f/41R/wjVz/0F7n/AL92/wD8arqqKLhc5X/hGrn/AKC9z/37t/8A41R/wjVz/wBBe5/792//AMarqqKLhc5X/hGrn/oL3P8A37t//jVH/CNXP/QXuf8Av3b/APxquqoouFzlf+Eauf8AoL3P/fu3/wDjVH/CNXP/AEF7n/v3b/8Axquqrm/GWp3WieENc1mxIFzYWNzcRbhkb4omdcjuMioqVOWLk+hUIuUlFdTFu7XTrC7t9PvvExtrq7YLDFI1qkkrHoEVowWJ9ADWp/wjVz/0F7n/AL92/wD8ar85P+CZfw58KeJ/gbpn7RHjGyTxF8R/Ft/qF1ea5qI+1XimK6khRYXl3eSoVeke3JJ7YA+9/Ffxq+EHgXX7Hwp4y8aaPoutanLHDbWV3fQw3MskpARViZg5LFhjjnIxW1SDg+SW5mpX1Wx0n/CNXP8A0F7n/v3b/wDxqj/hGrn/AKC9z/37t/8A41Wbf/FH4b6X43sPhpqPifTbbxZqiNLa6TJdRLezIqlyyQFt5G1WPToCe1d5UeZRyv8AwjVz/wBBe5/792//AMao/wCEauf+gvc/9+7f/wCNVo6/4j8P+FdMk1rxPqdrpGnwkB7i8mSCFSegLyFVGe3Nc/4U+J3w68deHbrxf4M8S6drmh2Lyxz3tncxz28TQDdKHkRioKDlsngUrgX/APhGrn/oL3P/AH7t/wD41R/wjVz/ANBe5/792/8A8ao8F+OvBnxH8PQeLPAOt2fiHRrousV5YzpcQO0bFXAdCRlWBBHY11VN3W4rnK/8I1c/9Be5/wC/dv8A/GqQ+G7gAk6vcAD/AGLf/wCNVV8UfEr4d+CLmGz8Z+J9M0K4uEMkUd9eQ2zugOCyrIykjPGRTvC/xG+H3jmW4tvBniXTdektVVpksbuG5MauSFLiNmwCQQM9aVx3KWkwaZr8LXGheKP7RiQ4Z7ZrWZQfQlIyK1v+Eauf+gvc/wDfu3/+NV+X37VPhTwv+z/+1L+z78TPhJp0fhfU/GniGTRdbi04C1ttRtZzEp8+CPEbMDIW3bck4JOQpH611ai+RT82vut/mhN2lbyv+f8Akcr/AMI1c/8AQXuf+/dv/wDGqP8AhGrn/oL3P/fu3/8AjVdVRU3Hc5X/AIRq5/6C9z/37t//AI1R/wAI1c/9Be5/792//wAarqqKLhc5X/hGrn/oL3P/AH7t/wD41R/wjVz/ANBe5/792/8A8arqqKLhc5X/AIRq5/6C9z/37t//AI1R/wAI1c/9Be5/792//wAarqqKLhc5X/hGrn/oL3P/AH7t/wD41R/wjVz/ANBe5/792/8A8arqqKLhc5X/AIRq5/6C9z/37t//AI1R/wAI1c/9Be5/792//wAarqqKLhc5X/hGrn/oL3P/AH7t/wD41R/wjVz/ANBe5/792/8A8arqqKLhc5X/AIRq5/6C9z/37t//AI1R/wAI1c/9Be5/792//wAarqqKLhc5X/hGrn/oL3P/AH7t/wD41R/wjVz/ANBe5/792/8A8arqqKLhc5X/AIRq5/6C9z/37t//AI1R/wAI1c/9Be5/792//wAarqqKLhc5X/hGrn/oL3P/AH7t/wD41R/wjVz/ANBe5/792/8A8arqqKLhc5X/AIRq5/6C9z/37t//AI1R/wAI1c/9Be5/792//wAarqqKLhc5X/hGrn/oL3P/AH7t/wD41R/wjVz/ANBe5/792/8A8arqqKLhc5X/AIRq5/6C9z/37t//AI1R/wAI1c/9Be5/792//wAarqqKLhc5X/hGrn/oL3P/AH7t/wD41R/wjVz/ANBe5/792/8A8arqqKLhc5X/AIRq5/6C9z/37t//AI1R/wAI1c/9Be5/792//wAarqqKLhc5X/hGrn/oL3P/AH7t/wD41R/wjVz/ANBe5/792/8A8arqqKLhc5X/AIRq5/6C9z/37t//AI1R/wAI1c/9Be5/792//wAarqqKLhc5X/hGrn/oL3P/AH7t/wD41R/wjVz/ANBe5/792/8A8arqqKLhc5X/AIRq5/6C9z/37t//AI1R/wAI1c/9Be5/792//wAarqqKLhc5X/hGrn/oL3P/AH7t/wD41R/wjVz/ANBe5/792/8A8arqqKLhc5X/AIRq5/6C9z/37t//AI1R/wAI1c/9Be5/792//wAarqqKLhc5X/hGrn/oL3P/AH7t/wD41R/wjVz/ANBe5/792/8A8arqqKLhc5X/AIRq5/6C9z/37t//AI1R/wAI1c/9Be5/792//wAarqqKLhc5X/hGrn/oL3P/AH7t/wD41R/wjVz/ANBe5/792/8A8arqqKLhc5X/AIRq5/6C9z/37t//AI1R/wAI1c/9Be5/792//wAarqqKLhc5UeGrkHP9r3P/AH7t/wD41Un/AAj1x/0Fbj/viD/43XTUUhH/0P38ooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACviz9tHx18a/CPw/vrP4eeHFm8MajYXMeu+IYT9svNFtmRhLNDpYMTXJEeSGEw2H5ijKDX2nXI/EC1mvfAXiSztozLNPpl5GiDkszwuAB9ScVjXdoN9vxNaGs0jxT9jvwh8M/A/wCzd4I0D4Q623iTwsto01tqTrse6aeRpJZGT/lmfMZgUPKY2nkZr5l/4KsaH4VH7KWq+KLhoLHxFpGq6Ze6VMEAuJbyKUKVQgbmItzIx7BUycBeJf8AgmHqFxq/7Cuh6X4XvYYdb0+TWbVTJiRba8a5meLzoxzgb0cqeSp969a8Jfs6/E/xr4c1eb9qnxTYeL/El5pV9otium2pt9OsLa/jMc06xNjzLqUEK0hVdqKEQDdIX6sanzycXtqn+KMsNZJc3o/yZtfs1fCj4Y6t4E8FfHTUNMsvEvjzxDYQazceJ7q0hOpSXOoW4MmyXBaKNEYwxxK21EG0dyfrqvib9j/9nr4tfAXwXp3hv4teOINftPC9rNp+j2mn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rdRJjkpKvyyLn2YH9K7igAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooA+S/2nvGeqtoE/gTwxbTXMkkf2jVJokJS2tU+YK7j5VLkZwf4R7iuP/Zd8Va34chj8J+JbOe30jWybjS7t0byWlPDxB/ujftyoznIPqK9a/aY8Sx6H8N59CswDqPiSVLSGMfeYFgZGx9AFz6kV674I8NR+F/Bmj+GJMSf2faxRPkDBdQCxx0+9k0AdbRRRQAUUUUAFFFFABRRRQAUUUUAFeM/Gvxrqfhnwu+keFraW+8RayrQ2kMCGSSNSMSTEKDgIDwTgbiPevZq8h+NXxBsvh14JvNSyDqd+jWtkg++8rggH1Kpncfy70AfFX7OnifxB4E8Qy6leWc7+F7+X7Ff3CozxW86co7lc7dpbDE8Yb2r9MlZXUOhDKwyCOQRXjXwF8D3HgX4a2Gm6im2+vS13cq3VXmwQh91UAH3zXsqqqKEQBVHAA4AFAC0UUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFAH//V/fyiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKjmiiuIngnQSRyKVZWGQykYII9CKkooA+L9Ivrz9mzx3caDq6O/gPxFP5lpc8lbOVuqt9Bww7qAw6EV9lwTwXUEdzbSLLFKoZHQhlZTyCCOCDWN4m8MaH4w0afQfEVol5ZXA+ZG6gjoynqrDsRyK+VBpXxR/Z4uZZPD8Uvi/wAEMSxtySbi0HU4wCQAOpAKnuFPNAH2TRXmPgH4veBviLbqdCv1jvMfPZzkR3CHv8pPzD3XIr06gAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKa7pEjSSMERRkknAAHcmgB1c94p8VaF4N0W41/xFdLa2luMkk/Mx7Kg6sx7AV4744/aI8KeH7gaH4RjbxXrsp2R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jN+/vnU9woB2/ko/2jXnujeDvFPj27/tDSfDupeKJnP/AB/axK0Vvn1C7hkfWRsj+GvuzwX8B/ht4I2XFlpi318vJurz99Ju9QG+VT/uqK9iACgKowB0FAHxdoP7PfxOuohFrXia28OWrD5rfR4BGcejOgjLfVmavQ9I/Za+F1jILnWI7rXLjqz3c7YJ7/LHtH55r6PooA47RPh74G8OBRomg2Voy9HSBN//AH2QW/WuwVVUBVAAHYUtFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAf/R/fyiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooA+d/jx8MpvEGmJ458IqbXxXoOJ4Zovlkmji5MZx1IHK5/wB3oa808d/tFf2h8HdOvPD5MHiDxBvtJFT71u0QAnde/O4CPv8ANkcivtIgEEEZBr87/hx8PNO/4aW1TQ5Yy9h4enuLyKNuQDuUw5B9N4P1AoA9g+BP7Pdj4Wtbfxb41t1utdlAkhhf5ktARkZHRpfUnO3oOea+saKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooA/9L9/KKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigArHtvD2g2erXOvWmnW8OpXihZ7lIlWaVRjAdwNzAYHU9hXj+p/tJ/CfR/jPo/wB1K+u7bxlryTPZ28thdRQTLBG0rlLh41icbUbBRmGRjOeK9B1X4jeD9F8daF8NtQ1BU8R+JILu6srQAs7wWQUzSMQCFUb1ALY3HgZwcHRPuD0djt6K8c0v48fDfWPjPq/wBs76X/hM9EsI9SuLZ4JEjNtLtw0cpGxyN65APH4HHsdC2uD3sFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQAUUUUAFFFFABRRRQB//9P9/KKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigD8sP+ChWv6N8L/jJ+zT8btbla2svDniW6tLuVUZ2FreRJ5uAgLMdsbYUDkmvLPFI8S+Cf21/gD8efivcHR9S+IVp4jS/t7iUC20fTba1EtnZ5Pyq8Ub7p2z80rPztC1+nnxV+Bngr4x654H1fxrEbuHwJq39s2tqQrQzXKwvFH5oYHKxswkAHVlXPGQea+N/7MPw0/aD8UeBPEPxJge/t/Ad3PewWJC/Z7qSZUAW4yCWjVo1YoCAxGGyuQXSfJZrfmb+TST/z/wCHHK0rp7W/FNtf1/kY/wAGdHn+Inj7WP2mNb019NGs2MejeHIJ4/LuF0KKUz/aplPzLJeynzFRuUiWMEKxcV9R0iqqKEQBVUYAHAAFLQ2tlsLzYUUUUgCiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigD/9T9/KKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooAKKKKACiiigAooooA//Z)

​			具体的操作流程：假设集合里面有3个元素{x, y, z}，哈希函数的个数为3。首先将位数组进行初始化，将里面每个位都设置位0。对于集合里面的每一个元素，将元素依次通过3个哈希函数进行映射，每次映射都会产生一个哈希值，这个值对应位数组上面的一个点，然后将位数组对应的位置标记为1。查询W元素是否存在集合中的时候，同样的方法将W通过哈希映射到位数组上的3个点。如果3个点的其中有一个点不为1，则可以判断该元素一定不存在集合中。反之，如果3个点都为1，则该元素可能存在集合中。注意：此处不能判断该元素是否一定存在集合中，可能存在一定的误判率。可以从图中可以看到：假设某个元素通过映射对应下标为4，5，6这3个点。虽然这3个点都为1，但是很明显这3个点是不同元素经过哈希得到的位置，因此这种情况说明元素虽然不在集合中，也可能对应的都是1，这是误判率存在的原因

##### 4、使用场景

- **google的guava包中有对Bloom Filter的实现**

- **通常使用布隆过滤器去解决redis中的缓存穿透，解决方案是redis中bitmap的实现，**

- **钓鱼网站、垃圾邮件检测**



### 阻塞队列

![](data:image/png;base64,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)

- ArrayBlockingQueue：一个由数组结构组成的有界阻塞队列。
- LinkedBlockingQueue：一个由链表结构组成的有界int最大值阻塞队列。
- PriorityBlockingQueue：一个支持优先级排序的无界阻塞队列。
- DelayQueue：一个使用优先级队列实现的无界阻塞队列。
- SynchronousQueue：一个不存储元素的阻塞队列。   ？？
- LinkedTransferQueue：一个由链表结构组成的无界阻塞队列。    ？？
- LinkedBlockingDeque：一个由链表结构组成的双向阻塞队列。  ？？

##### ArrayBlockingQueue详解

```java
//ArrayBlockingQueue，相信大家看名字就能猜到，该阻塞队列是基于数组实现的，必须制定大小且不可变，同时使用ReentrantLock来实现并发问题的解决。同时需要注意的是ArrayBlockingQueue只有一把锁，put和take操作会相互阻塞。
final ReentrantLock lock;

    /** Condition for waiting takes */
    private final Condition notEmpty;

    /** Condition for waiting puts */
    private final Condition notFull;

 public ArrayBlockingQueue(int capacity, boolean fair) {
        if (capacity <= 0)
            throw new IllegalArgumentException();
        this.items = new Object[capacity];
        lock = new ReentrantLock(fair);
        notEmpty = lock.newCondition();
        notFull =  lock.newCondition();
    }
```



##### LinkedBlockingQueue详解

```java
/*
LinkedBlockingQueue和ArrayBlockingQueue十分相似，其底层是借由链表实现。
除此之外，还有一个不同点，LinkedBlockingQueue拥有两个锁，因此put和take的线程可以同时运行，
阻塞队列LinkedBlockingQueue是一个单向链表实现的阻塞队列,先进先出的顺序并且是无界队列,默认使用int的最大值。
继承AbstractQueue,实现了BlockingQueue，Serializable接口。
插入和取出使用不同的锁，putLock插入锁,takeLock取出锁，注意不接受null,添加和删除数据的时候可以并行。
主要作为固定大小线程池(Executors.newFixedThreadPool())底层所使用的阻塞队列使用   
*/
static class Node<E> {
        E item;

        /**
         * One of:
         * - the real successor Node
         * - this Node, meaning the successor is head.next
         * - null, meaning there is no successor (this is the last node)
         */
        Node<E> next;

        Node(E x) { item = x; }
    }


    /** The capacity bound, or Integer.MAX_VALUE if none */
    private final int capacity;

    /** Current number of elements */
    private final AtomicInteger count = new AtomicInteger();

    /**
     * Head of linked list.
     * Invariant: head.item == null
     */
    transient Node<E> head;

    /**
     * Tail of linked list.
     * Invariant: last.next == null
     */
    private transient Node<E> last;

    /** Lock held by take, poll, etc */
    private final ReentrantLock takeLock = new ReentrantLock();

    /** Wait queue for waiting takes */
    private final Condition notEmpty = takeLock.newCondition();

    /** Lock held by put, offer, etc */
    private final ReentrantLock putLock = new ReentrantLock();

    /** Wait queue for waiting puts */
    private final Condition notFull = putLock.newCondition();


	public void put(E e) throws InterruptedException {
        if (e == null) throw new NullPointerException();
        // Note: convention in all put/take/etc is to preset local var
        // holding count negative to indicate failure unless set.
        int c = -1;
        Node<E> node = new Node<E>(e);
        final ReentrantLock putLock = this.putLock;
        final AtomicInteger count = this.count;
        putLock.lockInterruptibly();
        try {
            /*
             如果count 是计数。队列满了暂停put操作
             */
            while (count.get() == capacity) {
                notFull.await();
            }
            enqueue(node);
            c = count.getAndIncrement();
            if (c + 1 < capacity)
                notFull.signal();
        } finally {
            putLock.unlock();
        }
        if (c == 0)
            signalNotEmpty();
    }



```



##### DelayQueue的底层实现

  DelayQueue是一个**线程安全且无界的阻塞队列**，只有在延迟时间满足后才能获取队列中的元素，因此队列中的元素必须实现Delay接口，在创建元素时指定多久时间后才能从队列中获取该元素。Delay Queue的底层实现是使用了PriorityQueue+ReentrantLock来实现延迟获取功能。

```java
public E peek() {
        final ReentrantLock lock = this.lock;
        lock.lock();
        try {
            return q.peek();
        } finally {
            lock.unlock();
        }
}

public boolean offer(E e) {
    final ReentrantLock lock = this.lock;
    lock.lock();
    try {
      q.offer(e);
      if (q.peek() == e) {
        leader = null;
        available.signal();
      }
      return true;
    } finally {
      lock.unlock();
    }
}

    public void put(E e) {
        offer(e);
    }

    public boolean offer(E e, long timeout, TimeUnit unit) {
        return offer(e);
    }

    public E poll() {
        final ReentrantLock lock = this.lock;
        lock.lock();
        try {
            E first = q.peek();
            if (first == null || first.getDelay(NANOSECONDS) > 0)
                return null;
            else
                return q.poll();
        } finally {
            lock.unlock();
        }
    }

   //take这个方法算是个阻塞的获取对象
    public E take() throws InterruptedException {
        final ReentrantLock lock = this.lock;
        lock.lockInterruptibly();
        try {
          //死循环获取数据
            for (;;) {
                E first = q.peek();
                if (first == null)
                    //如果队列没有数据则等待 放入操作唤醒
                    available.await();
                else {
                  	//如果延迟到了直接取出
                    long delay = first.getDelay(NANOSECONDS);
                    if (delay <= 0)
                        return q.poll();
                    first = null; 
                  	//如果延迟没有到，则先把对象置为空，如果当前执行的线程不为空，等待其他线程put操作
                    if (leader != null)
                        available.await();
                    else {
                        Thread thisThread = Thread.currentThread();
                        leader = thisThread;
                        try {
                            available.awaitNanos(delay);
                        } finally {
                            if (leader == thisThread)
                                leader = null;
                        }
                    }
                }
            }
        } finally {
            if (leader == null && q.peek() != null)
                available.signal();
            lock.unlock();
        }
    }


    public E poll(long timeout, TimeUnit unit) throws InterruptedException {
        long nanos = unit.toNanos(timeout);
        final ReentrantLock lock = this.lock;
        lock.lockInterruptibly();
        try {
            for (;;) {
                E first = q.peek();
                if (first == null) {
                    if (nanos <= 0)
                        return null;
                    else
                        nanos = available.awaitNanos(nanos);
                } else {
                    long delay = first.getDelay(NANOSECONDS);
                    if (delay <= 0)
                        return q.poll();
                    if (nanos <= 0)
                        return null;
                    first = null; // don't retain ref while waiting
                    if (nanos < delay || leader != null)
                        nanos = available.awaitNanos(nanos);
                    else {
                        Thread thisThread = Thread.currentThread();
                        leader = thisThread;
                        try {
                            long timeLeft = available.awaitNanos(delay);
                            nanos -= delay - timeLeft;
                        } finally {
                            if (leader == thisThread)
                                leader = null;
                        }
                    }
                }
            }
        } finally {
            if (leader == null && q.peek() != null)
                available.signal();
            lock.unlock();
        }
    }

    
```

 



##### PriorityQueue分析

  其中PriorityQueue是种优先级队列，线程不安全，队列中的元素会按照优先级来排序。该队列底层实现是使用二叉堆，并且元素按照其自然顺序进行排序，或者根据构造队列时提供的Comparator进行排序。因为PriorityQueue中的元素都要进行比较，所以优先级队列中不能拥有null元素，也不能有不能比较的元素。

```java
public class PriorityQueue<E> extends AbstractQueue<E>
    implements java.io.Serializable {
    
    //队列的默认容量
    private static final int DEFAULT_INITIAL_CAPACITY = 11;
 
    //底层用于存放数据的数组
    transient Object[] queue; // non-private to simplify nested class access
 
    //队列中的元素数量计数
    private int size = 0;
 
    //比较器
    private final Comparator<? super E> comparator;
 
    //快速失败机制使用的变量
    transient int modCount = 0; 
 
    //创建一个默认容量的队列
    public PriorityQueue() {
        this(DEFAULT_INITIAL_CAPACITY, null);
    }
 
    //创建一个指定容量的队列
    public PriorityQueue(int initialCapacity) {
        this(initialCapacity, null);
    }
 
    //创建一个指定比较器的默认容量队列
    public PriorityQueue(Comparator<? super E> comparator) {
        this(DEFAULT_INITIAL_CAPACITY, comparator);
    }
 
    //创建一个指定比较器且指定容量队列
    public PriorityQueue(int initialCapacity,
                         Comparator<? super E> comparator) {
        //判断指定的容量值是否合法
        if (initialCapacity < 1)
            throw new IllegalArgumentException();
        this.queue = new Object[initialCapacity];    //初始化底层数组
        this.comparator = comparator;    //比较器初始化
    }
 
    //创建一个带有指定集合中的元素的队列
    @SuppressWarnings("unchecked")
    public PriorityQueue(Collection<? extends E> c) {
 
        //判断c是否是有序集合
        //若是有序集合，那么就以其比较器作为队列的比较器
        if (c instanceof SortedSet<?>) {
            SortedSet<? extends E> ss = (SortedSet<? extends E>) c;
            this.comparator = (Comparator<? super E>) ss.comparator();
            initElementsFromCollection(ss);
        }
 
        //判断集合是否是优先级队列
        //若是的话，直接使用该队列的比较器，
        else if (c instanceof PriorityQueue<?>) {
            PriorityQueue<? extends E> pq = (PriorityQueue<? extends E>) c;
            this.comparator = (Comparator<? super E>) pq.comparator();
            initFromPriorityQueue(pq);
        }
        else {
            this.comparator = null;
            initFromCollection(c);
        }
    }
 
    //将容器c中的元素添加到优先级队列中
    private void initElementsFromCollection(Collection<? extends E> c) {
        Object[] a = c.toArray();
        // If c.toArray incorrectly doesn't return Object[], copy it.
        if (a.getClass() != Object[].class)
            a = Arrays.copyOf(a, a.length, Object[].class);
        int len = a.length;
        if (len == 1 || this.comparator != null)
            for (int i = 0; i < len; i++)
                if (a[i] == null)
                    throw new NullPointerException();
        this.queue = a;
        this.size = a.length;
    }
 
    //将优先级队列c中的元素添加到当前优先级队列中
    private void initFromPriorityQueue(PriorityQueue<? extends E> c) {
        if (c.getClass() == PriorityQueue.class) {
            this.queue = c.toArray();
            this.size = c.size();
        } else {
            initFromCollection(c);
        }
    }
 
    //将容器c中的元素添加到优先级队列中
    private void initFromCollection(Collection<? extends E> c) {
        initElementsFromCollection(c);
        heapify();
    }
 
    //创建包含优先级队列c中元素的队列，且使用同一个比较器
    @SuppressWarnings("unchecked")
    public PriorityQueue(PriorityQueue<? extends E> c) {
        this.comparator = (Comparator<? super E>) c.comparator();
        initFromPriorityQueue(c);
    }
 
    //创建包含排序集合c中元素的优先级队列，且使用同一个比较器
    @SuppressWarnings("unchecked")
    public PriorityQueue(SortedSet<? extends E> c) {
        this.comparator = (Comparator<? super E>) c.comparator();
        initElementsFromCollection(c);
    }
}
```



### Fork/Join框架

https://www.cnblogs.com/senlinyang/p/7885964.html



### Java对象头

```xml
				<dependency>
            <groupId>org.openjdk.jol</groupId>
            <artifactId>jol-core</artifactId>
            <version>0.9</version>
        </dependency>

```

```java
public class JavaObjectHeadTest {

    public static void main(String[] args) {
      	//这边随便一个对象
        JavaObjectHead head = new JavaObjectHead();
        
        System.out.println(head.hashCode());//大端小端存储
        System.out.println(Integer.toHexString(head.hashCode()));//大端小端存储
        
        System.out.println(ClassLayout.parseInstance(head).toPrintable());
        synchronized (head) {
            System.out.println("加锁");
        }
    }
}

```

Java对象的对象头由 **mark word** 和 **klass pointer** 两部分组成，

- mark word存储了同步状态、标识、hashcode、GC状态等等。

- klass pointer存储对象的类型指针，该指针指向它的类元数据

值得注意的是，如果应用的对象过多，使用64位的指针将浪费大量内存。64位的JVM比32位的JVM多耗费50%的内存。我们现在使用的**64位 JVM会默认使用选项 +UseCompressedOops 开启指针压缩，将指针压缩至32位**。

 **==java对象头一共有5种状态 无锁、偏向锁、轻量锁、重量锁、GC 对应不同的Mark Word状态数据==**

```xml
|-----------------------------------------------------------------------------------------------------|
|                                    Object Header (128 bits)                                         |
|-----------------------------------------------------------------------------------------------------|
|                        Mark Word (64 bits)                                  |  Klass Word (64 bits) |
|-----------------------------------------------------------------------------------------------------|
|unused:25 | identity_hashcode:31 | unused:1 | age:4 | biased_lock:1 | lock:2 |OOP to metadata object |无锁
|--------------------------------------------------------------------|--------|-----------------------|
|thread:54 |     epoch:2          | unused:1 | age:4 | biased_lock:1 | lock:2 |OOP to metadata object |偏向锁
|--------------------------------------------------------------------|--------|-----------------------|
|             ptr_to_lock_record:62                                  | lock:2 |OOP to metadata object |轻量锁
|--------------------------------------------------------------------|--------|-----------------------|
|             ptr_to_heavyweight_monitor:62                          | lock:2 |OOP to metadata object |重量锁
|--------------------------------------------------------------------|--------|-----------------------|
|                                                                    | lock:2 |OOP to metadata object | GC
|-----------------------------------------------------------------------------------------------------|
```

**lock**: 锁状态标记位，该标记的值不同，整个mark word表示的含义不同。

**biased_lock**：偏向锁标记，为1时表示对象启用偏向锁，为0时表示对象没有偏向锁。

[![img](https://img2018.cnblogs.com/blog/1274378/201907/1274378-20190725164950583-2057732436.png)](https://img2018.cnblogs.com/blog/1274378/201907/1274378-20190725164950583-2057732436.png)

**age**：Java GC标记位对象年龄。

**identity_hashcode**：对象标识Hash码，采用延迟加载技术。当对象使用HashCode()计算后，并会将结果写到该对象头中。当对象被锁定时，该值会移动到线程Monitor中。

**thread**：持有偏向锁的线程ID和其他信息。这个线程ID并不是JVM分配的线程ID号，和Java Thread中的ID是两个概念。

**epoch**：偏向时间戳。

**ptr_to_lock_record**：指向栈中锁记录的指针。

**ptr_to_heavyweight_monitor**：指向线程Monitor的指针。









### LRU 和LFU  淘汰策略











