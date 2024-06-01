# Java开发-面试题-0004-HashMap 和 Hashtable的区别



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**







**HashMap** 和 **Hashtable **的区别

* 线程安全性

  * HashMap：**非线程安全。**在多线程环境下，不保证一致性。如果多个线程并发访问一个 HashMap，并且至少有一个线程修改了映射结构，必须自行同步。

  * Hashtable：**线程安全。**内部使用同步方法来保证线程安全，因此在多线程环境下可以安全使用。但由于同步开销较大，性能较低。

    ```java
    // Hashtable 里面的方法都使用了 synchronized 修饰，用于保证线程安全
    public synchronized int size() {...}
    public synchronized boolean contains(Object value) {...}
    public synchronized boolean containsKey(Object key) {...}
    public synchronized V get(Object key) {...}
    public synchronized V put(K key, V value) {...}
    ```

    

* 同步机制

  * HashMap：所有方法都没有进行同步操作。可以通过外部同步实现，比如使用 （java.util.Collections#synchronizedMap）**Collections.*synchronizedMap*(new HashMap<K,V>())** 方法；

    ```java
        // 1.里面是依赖于一个静态内部类，SynchronizedMap
    	// 2.将传过来的 Map 封装进这个静态内部类中
    	// 3，这个静态内部类里面的方法都是 synchronized 修饰的，所以可以保持线程安全
    	public static <K,V> Map<K,V> synchronizedMap(Map<K,V> m) {
            return new SynchronizedMap<>(m);
        }
    
    
        private static class SynchronizedMap<K,V>
            implements Map<K,V>, Serializable {
            
            public boolean containsKey(Object key) {
                synchronized (mutex) {return m.containsKey(key);}
            }
    
            public Set<K> keySet() {
                synchronized (mutex) {
                    if (keySet==null)
                        keySet = new SynchronizedSet<>(m.keySet(), mutex);
                    return keySet;
                }
            }
        }
    ```

  * Hashtable：所有方法都使用 **synchronized** 关键字进行了同步处理。

  

* 性能

  * HashMap：由于没有同步开销，性能较高。
  * Hashtable：由于每个方法都同步，性能较低。

  

* Null 值和 Null 键

  * HashMap：**允许一个 null 键和多个 null 值。**

  * Hashtable：**不允许 null 键和 null 值。**插入 null 键或 null 值会抛出 NullPOinterException。

    ```java
    // Hashtable 的 put 方法源码里面有标明
    
    // @exception  NullPointerException  if the key or value is <code>null</code>
    public synchronized V put(K key, V value) {
            // Make sure the value is not null
            if (value == null) {
                throw new NullPointerException();
            }
        ...
    }
    ```



* 继承体系

  * HashMap：继承自 **AbstractMap**，并实现了 Map 接口。
  * Hashtable：继承自 **Dictionary**，并实现了 Map 接口。

  

* 遍历方式

  * HashMap：通过 Iterator 进行遍历，使用 **fail-fast** 机制。如果在遍历时 HashMap 结构发生变化（**除通过 Iterator 的 remove 方法外**），会抛出 **ConcurrentModificationException**。
  * Hashtable：通过 Enumeration 进行遍历，不是 fail-fast 机制。

  

* 初始容量和扩容体系

  * HashMap：默认初始容量为 16，负载因子为 0.75。当容量达到初始容量的 75% 时会扩容。扩容机制是将容量翻倍，并重新哈希现有的键值对。（当知道要初始化HashMap的大小时，推荐使用：**com.google.common.collect.Maps#newHashMapWithExpectedSize**，自带优化容量算法，协助创建容量大小更合适的HashMap）

    ```java
        /**
         * The default initial capacity - MUST be a power of two.
         */
        static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16
    
        /**
         * The load factor used when none specified in constructor.
         */
        static final float DEFAULT_LOAD_FACTOR = 0.75f;
    ```

  * Hashtable：默认初始容量为 11，负载因子为 0.75。当容量达到初始容量的 75% 时会扩容。扩容机制是容量翻倍加1，并重新哈希现有的键值对。

  

* 存储结构

  * HashMap：使用哈希表实现， Java 8 以后，当桶中元素超过8个(即 **> 8**）时，会将**链表转换为红黑树**，以提高性能。当桶中元素数量减少到 6 个或更少（即**<= 6**）时，**红黑树会退化回链表**。（**数组 + 链表 + 红黑树**）

    ```java
        /**
         * The bin count threshold for using a tree rather than list for a
         * bin.  Bins are converted to trees when adding an element to a
         * bin with at least this many nodes. The value must be greater
         * than 2 and should be at least 8 to mesh with assumptions in
         * tree removal about conversion back to plain bins upon
         * shrinkage.
         */
        static final int TREEIFY_THRESHOLD = 8;
    
        /**
         * The bin count threshold for untreeifying a (split) bin during a
         * resize operation. Should be less than TREEIFY_THRESHOLD, and at
         * most 6 to mesh with shrinkage detection under removal.
         */
        static final int UNTREEIFY_THRESHOLD = 6;
    ```

    

  * Hashtable：使用哈希表实现，始终使用链表来处理冲突，没有红黑树优化。

  

* 使用场景

  * HashMap：适用于非线程安全的环境，特别是大多数单线程场景中，是最常用的 `Map` 实现。
  * Hashtable：适用于线程安全的环境，但由于其性能开销，通常推荐使用 **ConcurrentHashMap** 替代。





![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0004.jpeg?raw=true)



上图由 RealVisXL 生成









**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**

