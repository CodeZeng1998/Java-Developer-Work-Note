# 24-异常-DateTimeParseException



> **更多内容欢迎关注我（持续更新中，欢迎Star✨）**
>
> **Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**
>
> **（技术）微信公众号：CodeZeng1998**
>
> **（生活）微信公众号：好锅**
>
> **其他平台：CodeZeng1998**、**好锅**



<br/>



**问题描述**：日期格式转化异常



<br/>

**报错信息：**

```txt
2025-02-17 09:50:54.980 ERROR 19976 [TID: N/A] --- [nio-8080-exec-1] c.s.l.sdk.exception.BizExceptionHandler  : [lab-sdk]---未知异常==> method=POST requestURI=/location/exportList code=-1 msg=JSON parse error: Cannot deserialize value of type `java.time.LocalDate` from String "2025-02-13": Failed to deserialize java.time.LocalDate: (java.time.format.DateTimeParseException) Text '2025-02-13' could not be parsed at index 4
```



源码：

```java
    /**
     * 开始时间
     */
    @Schema(description = "开始时间")
    @DateTimeFormat(pattern = "yyyy/MM/dd")
    @JsonFormat(pattern = "yyyy/MM/dd", timezone = "GMT+8")
    private LocalDate startTime;


    /**
     * 结束时间
     */
    @Schema(description = "结束时间")
    @DateTimeFormat(pattern = "yyyy/MM/dd")
    @JsonFormat(pattern = "yyyy/MM/dd", timezone = "GMT+8")
    private LocalDate endTime;
```



传入参数：

```json
{
  "startTime": "2025-02-13",
  "endTime": "2025-02-17"
}
```



<br/>

**错误原因**：报错原因很明显，出现 DateTimeParseException，从给出的错误日志来看，这是一个 JSON 解析错误，具体问题在于尝试把字符串 `"2025-02-13"` 反序列化为 `java.time.LocalDate` 类型时失败了。





**可能的原因：**

1. **日期格式不匹配**：Jackson 默认采用的日期格式可能和输入的 `"2025-02-13"` 格式不相符，所以无法正确解析。
2. **缺少日期格式化配置**：在项目里没有对 `LocalDate` 类型配置合适的日期格式化方式。



<br/>

**解决方案**：传入参数和接受参数的日期格式统一即可。





> **更多内容欢迎关注我（持续更新中，欢迎Star✨）**
>
> **Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**
>
> **（技术）微信公众号：CodeZeng1998**
>
> **（生活）微信公众号：好锅**
>
> **其他平台：CodeZeng1998**、**好锅**



