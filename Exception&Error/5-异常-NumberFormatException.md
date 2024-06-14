# 5-异常-NumberFormatException



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



问题描述：NumberFormatException，数据类型转换错误



问题代码：其实这里我已经知道了传过来的字符串是带有小数点的数字类型的数据了，只是没想到后面转换的时候竟然忘记了，然后粗心大意导致出现了这个异常。

```java
                        double average =
                                oneClassNoiseDayDataList.stream()
                                        .filter(
                                                item ->
                                                        StringUtils.isNotBlank(item.getData())
                                                                && NumberUtil.isNumber(
                                                                        item.getData()))
                                        .map(NoiseDayData::getData)
                                        .map(Integer::valueOf)
                                        .mapToInt(Integer::intValue)
                                        .average()
                                        .orElse(Double.NaN);
```



报错信息：

```
java:149) - [Error]Kafka Consumer, XXX Topic, execution encountered an exception, cost time 1 s
java.lang.NumberFormatException: For input string: "46.9"
	at java.lang.NumberFormatException.forInputString(NumberFormatException.java:65) ~[?:1.8.0_333]
	at java.lang.Integer.parseInt(Integer.java:580) ~[?:1.8.0_333]
	at java.lang.Integer.valueOf(Integer.java:766) ~[?:1.8.0_333]
```



问题原因：粗心导致的，这个问题还是很容易解决的。

解决方案：

```java
                        double average =
                                oneClassNoiseDayDataList.stream()
                                        .filter(
                                                item ->
                                                        StringUtils.isNotBlank(item.getData())
                                                                && NumberUtil.isNumber(
                                                                        item.getData()))
                                        .map(NoiseDayData::getData)
                                        .map(Double::parseDouble)
                                        .mapToDouble(Double::doubleValue)
                                        .average()
                                        .orElse(Double.NaN);
```







![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Exception&Error/image/5-%E5%BC%82%E5%B8%B8-NumberFormatException.png?raw=true)

上图是由 Pic 生成的

关键词：A hesitant womant sat by the beach smoking cigarettes and drinking, watching the sunset





**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**





