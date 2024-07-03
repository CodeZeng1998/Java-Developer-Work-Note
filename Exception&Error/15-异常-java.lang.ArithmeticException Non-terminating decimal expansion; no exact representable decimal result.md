# 15-异常-java.lang.ArithmeticException Non-terminating decimal expansion; no exact representable decimal result



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



<br/>



**问题描述**：使用 **BigDecimal** 进行数据相除时，出现报错：java.lang.ArithmeticException: Non-terminating decimal expansion; no exact representable decimal result.

<br/>

**原代码如下**：

```java
String oldValue ="1";
String newValue = "1";

// 正确的数据（1）均为数值类型；（2）newValue由于要当做被除数，所以不能为 0；（3）oldValue 小于 newValue 的5分之一
boolean rightData =  NumberUtil.isNumber(oldValue)
    && NumberUtil.isNumber(newValue)
    && !NumberUtil.equals(new BigDecimal(newValue), BigDecimal.valueOf(0))
    && NumberUtil.isLess(
    new BigDecimal(oldValue),
    new BigDecimal(oldValue)
    .divide(
        BigDecimal.valueOf(5)));
```





<br/>

**报错信息：**

```txt
java.lang.ArithmeticException: Non-terminating decimal expansion; no exact representable decimal result.
	at java.math.BigDecimal.divide(BigDecimal.java:1693) ~[?:1.8.0_311]
```



<br/>

**错误原因**：在执行过程中，遇到了一个无法准确表示的小数结果引发的`java.lang.ArithmeticException: Non-terminating decimal expansion; no exact representable decimal result`。是由于使用`BigDecimal.divide`时，没有指定舍入模式，导致除法运算产生了一个无限小数。



<br/>

**解决方案**：使用了`divide`方法的重载版本，并指定了舍入模式（`RoundingMode.ROUND_HALF_UP`）（舍入模式按需选择）和小数位数。这样可以确保即使除法运算结果是一个无限小数，也能得到一个近似值而不会抛出异常。



<br/>

**修改后的代码**：

```java
String oldValue ="1";
String newValue = "1";

// 正确的数据（1）均为数值类型；（2）newValue由于要当做被除数，所以不能为 0；（3）oldValue 小于 newValue 的5分之一(相除采用四舍五入的模式，保留两位小数)
boolean rightData =  NumberUtil.isNumber(newValue)
    && !NumberUtil.equals(new BigDecimal(newValue), BigDecimal.valueOf(0))
    && NumberUtil.isLess(
    new BigDecimal(oldValue),
    new BigDecimal(oldValue)
    .divide(
        BigDecimal.valueOf(5), 2,BigDecimal.ROUND_HALF_UP));
```

<br/>



![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Exception&Error/image/15-%E5%BC%82%E5%B8%B8-java.lang.ArithmeticException%20Non-terminating%20decimal%20expansion;%20no%20exact%20representable%20decimal%20result.png?raw=true)

上图是由 Pic 生成的

关键词：Seeing tigers and lions in the sea

<br/>



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**





