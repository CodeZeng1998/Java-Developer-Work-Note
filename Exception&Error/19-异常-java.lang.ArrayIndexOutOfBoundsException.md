# 19-异常-java.lang.ArrayIndexOutOfBoundsException



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**



<br/>



**问题描述**：今天发现项目日志里面有一些报错，java.lang.ArrayIndexOutOfBoundsException。



<br/>

**报错信息：**

```txt
2024-07-11 10:59:49.611 ERROR 7736 --- [.0-8027-exec-10]java.lang.ArrayIndexOutOfBoundsException: 1
...
...
...
	at java.lang.Thread.run(Thread.java:748) [?:1.8.0_311]
```



<br/>

原代码如下：

```java
    public String getXxxString(String token) {
        if (StringUtils.isNotBlank(token)) {
            token = token.split(" ")[1];
        }
        return token;
    }
```





<br/>

**错误原因**：使用字符串分割后生成对应的字符串数组，没有对字符串的长度进行判断即直接获取对应位置的数据，导致出现数组索引超过边界值的报错。



<br/>

**解决方案**：获取数据之前判断一下该位置是否存在。例如：

```java
public String getXxxString(String token) {
    if (StringUtils.isNotBlank(token)) {
        String[] parts = token.split(" ");
        if (parts.length > 1) {
            token = parts[1];
        }
    }
    return token;
}
```





![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Exception&Error/image/19-%E5%BC%82%E5%B8%B8-java.lang.ArrayIndexOutOfBoundsException.png?raw=true)

上图是由 Pic 生成的

关键词：If you don't manage your finances, wealth won't leave you png

<br/>



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**





