# 4-异常-log4j配置日志打印滚动覆盖出现日志丢失问题(附源码分析)



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



问题描述：**服务经常性的出现某些时刻数据丢失问题。**

问题原因：**日志配置了同一天日志文件的数量最大值，日志打印的滚动打印配置。所以当日志比较多的时候，超过这个最大数量的日志会滚动覆盖出现日志丢失问题。**

解决方案：**根据服务具体的日志需求，设置 DefaultRolloverStrategy  标签的 max 属性的值，找出一个比较适合的值。**





**log4j配置如下：**

* **重点查看 Appenders 里面的 DefaultRolloverStrategy 标签的属性；**
* **DefaultRolloverStrategy 的 max 是配置滚动文件的数量，默认的最大值为 7；**

```xml
<?xml version="1.0" encoding="UTF-8"?>

<Configuration status="WARN">
    <Properties>
        <Property name="baseDir">../logs</Property>
        <Property name="appCode">appCode</Property>
        <Property name="PID">????</Property>
        <Property name="LOG_EXCEPTION_CONVERSION_WORD">%xwEx</Property>
        <Property name="LOG_LEVEL_PATTERN">%5p</Property>
        <Property name="CONSOLE_LOG_PATTERN">%clr{%d{yyyy-MM-dd HH:mm:ss.SSS}}{faint} %clr{${LOG_LEVEL_PATTERN}} %clr{${sys:PID}}{magenta} %clr{---}{faint} %clr{[%15.15t]}{faint} %clr{%l}{cyan} %clr{:}{faint}%m%n${sys:LOG_EXCEPTION_CONVERSION_WORD}
        </Property>
        <Property name="FILE_LOG_PATTERN">%d{yyyy-MM-dd HH:mm:ss.SSS} ${LOG_LEVEL_PATTERN} ${sys:PID} --- [%t] %-40.40c{1.} : %m%n${sys:LOG_EXCEPTION_CONVERSION_WORD}
        </Property>
    </Properties>
    <Appenders>
        <Console name="Console" target="SYSTEM_OUT" follow="true">
            <PatternLayout pattern="${sys:CONSOLE_LOG_PATTERN}"/>
        </Console>
        <RollingFile name="RollingFileInfo" fileName="${baseDir}/${appCode}.log"
                     filePattern="${baseDir}/${appCode}/$${date:yyyy-MM}/${appCode}-%d{yyyy-MM-dd}-%i.log">
            <!--控制台只输出level及以上级别的信息（onMatch），其他的直接拒绝（onMismatch）-->
            <ThresholdFilter level="info" onMatch="ACCEPT" onMismatch="DENY"/>
            <PatternLayout pattern="[%d{HH:mm:ss:SSS}] [%p] - %l - %m%n"/>
            <Policies>
                <TimeBasedTriggeringPolicy/>
                <SizeBasedTriggeringPolicy size="100 MB"/>
            </Policies>
            <DefaultRolloverStrategy>
                <Delete basePath="${baseDir}" maxDepth="3">
                    <IfFileName glob="${appCode}/*/*.log" />
                    <IfLastModified age="365d" />
                </Delete>
            </DefaultRolloverStrategy>
        </RollingFile>

    </Appenders>
    <Loggers>
        <logger name="org.springframework" level="debug"/>
        <root level="debug">
            <AppenderRef ref="Console"/>
            <AppenderRef ref="RollingFileInfo"/>
        </root>
    </Loggers>
</Configuration>

```





源码如下：

* **DEFAULT_WINDOW_SIZE = 7**
* **由下列的build()方法可知，创建时假若没有设置 max 的值，则使用 DEFAULT_WINDOW_SIZE 的值，这个值为 7；**

```java
@Plugin(name = "DefaultRolloverStrategy", category = Core.CATEGORY_NAME, printObject = true)
public class DefaultRolloverStrategy extends AbstractRolloverStrategy {

    private static final int MIN_WINDOW_SIZE = 1;
    private static final int DEFAULT_WINDOW_SIZE = 7;

    /**
     * Builds DefaultRolloverStrategy instances.
     */
    public static class Builder implements org.apache.logging.log4j.core.util.Builder<DefaultRolloverStrategy> {
        @PluginBuilderAttribute("max")
        private String max;

        @PluginBuilderAttribute("min")
        private String min;

        @PluginBuilderAttribute("fileIndex")
        private String fileIndex;

        @PluginBuilderAttribute("compressionLevel")
        private String compressionLevelStr;

        @PluginElement("Actions")
        private Action[] customActions;

        @PluginBuilderAttribute(value = "stopCustomActionsOnError")
        private boolean stopCustomActionsOnError = true;

        @PluginBuilderAttribute(value = "tempCompressedFilePattern")
        private String tempCompressedFilePattern;

        @PluginConfiguration
        private Configuration config;

        @Override
        public DefaultRolloverStrategy build() {
            int minIndex;
            int maxIndex;
            boolean useMax;

            if (fileIndex != null && fileIndex.equalsIgnoreCase("nomax")) {
                minIndex = Integer.MIN_VALUE;
                maxIndex = Integer.MAX_VALUE;
                useMax = false;
            } else {
                useMax = fileIndex == null ? true : fileIndex.equalsIgnoreCase("max");
                minIndex = MIN_WINDOW_SIZE;
                if (min != null) {
                    minIndex = Integer.parseInt(min);
                    if (minIndex < 1) {
                        LOGGER.error("Minimum window size too small. Limited to " + MIN_WINDOW_SIZE);
                        minIndex = MIN_WINDOW_SIZE;
                    }
                }
                maxIndex = DEFAULT_WINDOW_SIZE;
                if (max != null) {
                    maxIndex = Integer.parseInt(max);
                    if (maxIndex < minIndex) {
                        maxIndex = minIndex < DEFAULT_WINDOW_SIZE ? DEFAULT_WINDOW_SIZE : minIndex;
                        LOGGER.error("Maximum window size must be greater than the minimum windows size. Set to " + maxIndex);
                    }
                }
            }
            final int compressionLevel = Integers.parseInt(compressionLevelStr, Deflater.DEFAULT_COMPRESSION);
            // The config object can be null when this object is built programmatically.
            final StrSubstitutor nonNullStrSubstitutor = config != null ? config.getStrSubstitutor() : new StrSubstitutor();
			return new DefaultRolloverStrategy(minIndex, maxIndex, useMax, compressionLevel, nonNullStrSubstitutor,
                    customActions, stopCustomActionsOnError, tempCompressedFilePattern);
        }
        
        ...
            
            
}
```





![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Exception&Error/image/4-%E5%BC%82%E5%B8%B8-log4j%E9%85%8D%E7%BD%AE%E6%97%A5%E5%BF%97%E6%89%93%E5%8D%B0%E6%BB%9A%E5%8A%A8%E8%A6%86%E7%9B%96%E5%87%BA%E7%8E%B0%E6%97%A5%E5%BF%97%E4%B8%A2%E5%A4%B1%E9%97%AE%E9%A2%98(%E9%99%84%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90).png?raw=true)

上图是由 Pic 生成的

关键词：A hesitant Tom cat sat by the beach smoking cigarettes and drinking, watching the sunset





**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**





