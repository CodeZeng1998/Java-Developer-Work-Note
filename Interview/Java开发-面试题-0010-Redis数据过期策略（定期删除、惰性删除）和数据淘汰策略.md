# Java开发-面试题-0010-Redis数据过期策略（定期删除、惰性删除）和数据淘汰策略



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**





Redis对数据设置数据的有效时间，数据过期以后，就需要将数据从内存中删除掉。可以按照不同的规则进行删除，这种删除规则就被称之为数据的**删除策略（数据过期策略）。惰性删除、定期删除。**

<br/>

## 惰性删除

惰性删除：设置该key过期时间后，我们不去管它，当需要该key时，我们在检查其是否过期，如果过期，我们就删掉它，反之返回该key。

* 优点：对CPU友好，只会在使用该key时才会进行过期检查，对于很多用不到的key不用浪费时间进行过期检查。
* 缺点：对内存不友好，如果一个key已经过期，但是一直没有使用，那么该key就会一直存在内存中，内存永远不会释放。



<br/>

## 定期删除

定期删除：每隔一段时间，我们就对一些key进行检查，删除里面过期的key(从一定数量的数据库中取出一定数量的随机key进行检查，并删除其中的过期key)。

定期清理有两种模式：

* **SLOW模式**是定时任务，执行频率默认为10hz，每次不超过25ms，以通过修改配置文件redis.conf 的hz 选项来调整这个次数。
* **FAST模式**执行频率不固定，但两次间隔不低于2ms，每次耗时不超过1ms

* 优点：可以通过限制删除操作执行的时长和频率来减少删除操作对 CPU 的影响。另外定期删除，也能有效释放过期键占用的内存。
* 缺点：难以确定删除操作执行的时长和频率。



```shell
# 在 redis.conf 文件中找到并修改以下配置项
hz 20
```



<br/>

Redis的过期删除策略：**惰性删除+ 定期删除**两种策略进行配合使用



<br/>

**数据淘汰策略**：当Redis中的内存不够用时，此时在向Redis中添加新的key，那么Redis就会按照某一种规则将内存中的数据删除掉，这种数据的删除规则被称之为内存的淘汰策略。



<br/>

Redis支持**8种不同策略**来选择要删除的key：

* **noeviction**： 不淘汰任何key，但是内存满时不允许写入新数据，默认就是这种策略。
* **volatile-ttl**： 对设置了TTL的key，比较key的剩余TTL值，TTL越小越先被淘汰
* **allkeys-random**：对全体key ，随机进行淘汰。
* **volatile-random**：对设置了TTL的key ，随机进行淘汰。
* **allkeys-lru**： 对全体key，基于LRU算法进行淘汰
* **volatile-lru**： 对设置了TTL的key，基于LRU算法进行淘汰
* **allkeys-lfu**： 对全体key，基于LFU算法进行淘汰
* **volatile-lfu**： 对设置了TTL的key，基于LFU算法进行淘汰



<br/>

算法说明：

* **LRU（Least  Recently Used）最近最少使用**。用当前时间减去最后一次访问时间，这个值越大则淘汰优先级越高。
* **LFU（Least Frequently Used）最少频率使用**。会统计每个key的访问频率，值越小淘汰优先级越高。



<br/>

数据淘汰策略-使用建议：

* 优先使用 allkeys-lru 策略。充分利用 LRU 算法的优势，把最近最常访问的数据留在缓存中。如果业务有明显的冷热数据区分，建议使用。
* 如果业务中数据访问频率差别不大，没有明显冷热数据区分，建议使用 allkeys-random，随机选择淘汰。
* 如果业务中有置顶的需求，可以使用 volatile-lru 策略，同时置顶数据不设置过期时间，这些数据就一直不被删除，会淘汰其他设置过期时间的数据。
* 如果业务中有短时高频访问的数据，可以使用 allkeys-lfu 或 volatile-lfu 策略。



<br/>

关于数据淘汰策略其他的面试问题：

1.数据库有1000万数据 ,Redis只能缓存20w数据, 如何保证Redis中的数据都是热点数据 ? 

使用**allkeys-lru(**挑选最近最少使用的数据淘汰)淘汰策略，留下来的都是经常访问的热点数据



2.Redis内存用完了会发生什么？

主要看数据淘汰策略是什么？如果是默认的配置（ noeviction ），会直接报错



<br/>























以上就是本文相关的所有内容了，如果发现有误欢迎评论指正，更多内容欢迎各位关注。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0010.png?raw=true)

上图是由 Pic 生成的

关键词：Riding a motorcycle with a swim ring by the seaside at sunset

<br/>

**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**

