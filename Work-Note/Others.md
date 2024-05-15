# --------------------------------------------------

# Java 映射工具

# 1. Map Struct

官网：https://mapstruct.org/

maven依赖

```xml

<!--mapStruct依赖 高性能对象映射-->
<!--mapstruct核心-->
<dependency>
    <groupId>org.mapstruct</groupId>
    <artifactId>mapstruct</artifactId>
    <version>1.5.0.Beta1</version>
</dependency>
<!--mapstruct编译-->
<dependency>
    <groupId>org.mapstruct</groupId>
    <artifactId>mapstruct-processor</artifactId>
    <version>1.5.0.Beta1</version>
</dependency>
```

样例代码

```java
public class Car {
    

    private Long id;
    private String name;
    private CarTypeEnum carType;
    
    //constructor, getters, setters etc.
}


public class CarVO {
 
    private Sting ignoreName;
    private Long carId;
    private String name;
    private String carType;
 
    //constructor, getters, setters etc.
}


import org.mapstruct.Mapper;
import org.mapstruct.Mapping;
import org.mapstruct.Mappings;

@Mapper
public interface NotificationConvert {

    NotificationConvert INSTANCE = Mappers.getMapper(NotificationConvert.class);

    @Mappings({
        	// 忽略属性
            @Mapping(target = "ignoreName", ignore = true), 
        	// 属性名称不同，配置映射
            @Mapping(target = "carId", source = "id"),
        	// 调用枚举类里的方法进行数据转换
            @Mapping(target = "carType", source = "carType", expression = "java(com.xxx.CarTypeEnum.getNameByCode(car.getCode()))")
    })
    CarVO convertPO2VO(Car car);
}
```





# --------------------------------------------------



# 1.FFmpeg

官网：https://ffmpeg.org/

## （1）图片或base64编码图片转mp4视频

项目配置信息

```yaml
# ffmpeg相关
ffmpeg:
 # docker ffmpeg 外部图片转视频文件夹路径 （挂载出来的文件夹）
 ffmpegPicture2VideoOutsideDockerPath: /ffmpeg/picture2video
 # docker ffmpeg 内部图片转视频文件夹路径
 ffmpegPicture2VideoInDockerPath: /img/picture2video
 # 图片合成视频的视频文件名称
 picture2VideoName: picture2video.mp4
 # 图片合成视频脚本路径
 picture2VideoShellPath: /ffmpeg/picture2video.sh
```

Java服务器调用脚本文件

```java
            ProcessBuilder pb = new ProcessBuilder("/bin/bash", picture2VideoShellPath, ffmpegPicture2VideoInDockerPath, date, uuid, picture2VideoName, suffix);
            pb.redirectErrorStream(true);
            Process process = pb.start();
            process.waitFor();
            process.destroy();
```

脚本文件（这边借用docker部署了FFmpeg，容器挂载一个目录出来使用）

docker_ffmpeg_container_id：需要替换成项目部署服务器上的Docker里的FFmpeg容器的container id

```shell
#!/bin/bash
ffmpegPicture2VideoInDockerPath=$1
date=$2
uuid=$3
picture2VideoName=$4
suffix=$5

docker exec docker_ffmpeg_container_id /bin/bash -c "ffmpeg -framerate 1 -f image2 -i ${ffmpegPicture2VideoInDockerPath}/${date}/${uuid}/image%04d${suffix} -c:v libx264 -pix_fmt yuv420p -crf 23 -profile:v baseline -level:v 3.0 -r 15 -s 1080x1920 ${ffmpegPicture2VideoInDockerPath}/${date}/${uuid}/${picture2VideoName}"

```

参数含义如下：

```xml
-c:v libx264：指定视频编码器为libx264，这是一种常用的H.264编码器。libx264可以提供高质量的视频压缩。

-pix_fmt yuv420p：指定像素格式为yuv420p。这是一种广泛支持的像素格式，适用于大多数播放器和设备。

-crf 23：指定常数率因子（Constant Rate Factor，CRF）为23。CRF是一种视频质量控制方法，23是一个中等的质量设置。较低的CRF值表示更高的质量，但也会导致更大的文件大小。

-profile:v baseline：指定H.264编码的配置文件为baseline。baseline配置文件是最基本的配置，广泛支持各种播放器和设备。

-level:v 3.0：指定H.264编码的级别为3.0。这是H.264编码的一个常用级别，适用于大多数设备。

-r 15：指定输出视频的帧率为15帧每秒。该值决定了视频播放的流畅度和速度。

-s 1080x1920：指定输出视频的分辨率为1080x1920，表示横向分辨率为1080像素，纵向分辨率为1920像素。这是竖屏的16:9纵向分辨率。
```



# --------------------------------------------------

# 分布式文件存储

# 1.MinIO

官网：https://www.minio.org.cn/














# --------------------------------------------------

# HTTP调用框架



# 1.Forset

官网：https://forest.dtflyx.com/

maven依赖

```xml
<!-- Forest HTTP 调用 API 框架 -->
<dependency>
    <groupId>com.dtflys.forest</groupId>
    <artifactId>forest-spring-boot-starter</artifactId>
    <version>1.5.32</version>
</dependency>

<!-- XML 框架依赖 -->
<dependency>
    <groupId>com.dtflys.forest</groupId>
    <artifactId>forest-jaxb</artifactId>
    <version>1.5.32</version>
</dependency>

```

```yaml
forest:
  backend: okhttp3             # 后端HTTP框架（默认为 okhttp3）
  max-connections: 500         # 连接池最大连接数（默认为 500）
  max-route-connections: 500   # 每个路由的最大连接数（默认为 500）
  max-request-queue-size: 100  # [自v1.5.22版本起可用] 最大请求等待队列大小
  max-async-thread-size: 300   # [自v1.5.21版本起可用] 最大异步线程数
  max-async-queue-size: 16     # [自v1.5.22版本起可用] 最大异步线程池队列大小
  connect-timeout: 3000        # 连接超时时间，单位为毫秒（默认为 timeout）
  read-timeout: 3000           # 数据读取超时时间，单位为毫秒（默认为 timeout）
  max-retry-count: 0           # 请求失败后重试次数（默认为 0 次不重试）
  ssl-protocol: TLS            # 单向验证的HTTPS的默认TLS协议（默认为 TLS）
  log-enabled: true            # 打开或关闭日志（默认为 true）
  log-request: true            # 打开/关闭Forest请求日志（默认为 true）
  log-response-status: true    # 打开/关闭Forest响应状态日志（默认为 true）
  log-response-content: false  # 打开/关闭Forest响应内容日志（默认为 false）
  async-mode: platform		  # 设置异步模式为平台模式
```



```java
@SpringBootApplication
@Configuration
@ForestScan(basePackages = "com.xxx.forset")
public class xxApp {
 ...
}


public interface MyClient {

    @Get("http://localhost:8080/hello")
    String helloForest();

}


@Component
public class MyService {
    
    // 注入自定义的 Forest 接口实例
    @Resource
    private MyClient myClient;

    public void testClient() {
        // 调用自定义的 Forest 接口方法
        // 等价于发送 HTTP 请求，请求地址和参数即为 helloForest 方法上注解所标识的内容
        String result = myClient.helloForest();
        // result 即为 HTTP 请求响应后返回的字符串类型数据
        System.out.println(result);
    }

}

```





# --------------------------------------------------

# 文档预览

# 1.kkFileView

网址：http://kkfileview.keking.cn/zh-cn/index.html

