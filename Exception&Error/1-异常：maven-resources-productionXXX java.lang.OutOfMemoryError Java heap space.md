# 1-异常：maven-resources-production:XXX: java.lang.OutOfMemoryError: Java heap space

一般出现[OOM](https://so.csdn.net/so/search?q=OOM&spm=1001.2101.3001.7020)，只需要设置一些对应软件的内存大小即可，但是这次遇见了修改内存大小无用，仔细看报错如下图所示：

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Exception&Error/image/1-1.png?raw=true)

错误显示是Build Failed，所以这个时候我们只需Rebuild一下即可解决问题。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Exception&Error/image/1-2.png?raw=true)