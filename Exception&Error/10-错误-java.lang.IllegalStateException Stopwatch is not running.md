# 10-错误-java.lang.IllegalStateException Stopwatch is not running



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



<br/>



**问题描述**：代码运行出现如下报错，使用 **org.apache.commons.lang3.time.StopWatch.stop** 的 **stop**方法出现报错。



<br/>

**报错信息：**

```txt
[16:08:56:257] [ERROR] - com.XXX.utils.XXX.printErrorLog(XXX.java:25) - 系统发生错误，信息如下:
java.lang.IllegalStateException: Stopwatch is not running. 
	at org.apache.commons.lang3.time.StopWatch.stop(StopWatch.java:255) ~[commons-lang3-3.9.jar:3.9]
...
...
...
[16:08:56:260] [ERROR] - com.XXX.utils.XXX.printErrorLog(XXX.java:28) - org.apache.commons.lang3.time.StopWatch.stop(StopWatch.java:255)
```



<br/>

**错误原因**：**org.apache.commons.lang3.time.StopWatch.stop** 的 未调用 **start** 方法就直接调用 **stop** 方法，出现上述报错。

**org.apache.commons.lang3.time.StopWatch.stop** 的源码如下：

```java
    public void stop() {
        if (this.runningState != StopWatch.State.RUNNING && this.runningState != StopWatch.State.SUSPENDED) {
            throw new IllegalStateException("Stopwatch is not running. ");
        } else {
            if (this.runningState == StopWatch.State.RUNNING) {
                this.stopTime = System.nanoTime();
            }

            this.runningState = StopWatch.State.STOPPED;
        }
    }
```



<br/>

**解决方案**：在调用 **org.apache.commons.lang3.time.StopWatch.stop** 的 **stop**方法前，需要先调用 **start** 方法。



<br/>



![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Exception&Error/image/10-%E9%94%99%E8%AF%AF-java.lang.IllegalStateException%20Stopwatch%20is%20not%20running.png?raw=true)

上图是由 Pic 生成的

关键词：A majestic eagle soaring above a dense forest

<br/>



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**





