# Java开发-实际工作经验和技巧-0005-使用MapStruct进行两个实体类的转换,出现所有属性值都为null的情况

**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**

<br/>



问题描述：两个实体类的属性名称完全一致，使用 `MapStruct`对数据进行自动转换时并未配置对应的映射，完全依赖于自动映射，出现了如下的情况，自动生成的转化实现类并未对对应的属性设置值，导致转换后的所有的属性都为 **null**。



<br/>

代码如下：

```java
package com.xxx;

import java.io.Serializable;
import java.sql.Timestamp;

public class UserVO implements Serializable {
    private static final long serialVersionUID = 1L;
    private String id;

    private Timestamp monitorTime;

    private Timestamp createTime;

    private Timestamp updateTime;

    private Integer flag;
}
```

<br/>

```java

package com.xxx;

import lombok.Data;
import java.io.Serializable;
import java.sql.Timestamp;


@Data
public class UserPO implements Serializable {
    @TableField(exist = false)
    private static final long serialVersionUID = 1L;
    private String id;

    private Timestamp monitorTime;

    private Timestamp createTime;

    private Timestamp updateTime;

    private Integer flag;
}
```

<br/>

```java
package com.xxx;

import com.xxx.UserPO;
import com.xxx.UserVO;

import org.mapstruct.Mapper;
import org.mapstruct.factory.Mappers;

@Mapper
public interface UserConvert {
    SurfaceWaterHourDataConvert INSTANCE = Mappers.getMapper(SurfaceWaterHourDataConvert.class);

    UserVO convertUserPo2UserVO(
            UserPO userPo);
}
```

<br/>

**自动生成的转化实现类**如下：

<br/>

```java

package com.xxx;


import javax.annotation.Generated;

@Generated(
    value = "org.mapstruct.ap.MappingProcessor",
    date = "2024-06-28T15:24:13+0800",
    comments = "version: 1.5.0.Beta1, compiler: javac, environment: Java 1.8.0_311 (Oracle Corporation)"
)
public class UserConvertImpl implements UserConvert {

    @Override
    public UserVO convertUserPo2UserVO(UserPO userPo) {
        if ( userPo == null ) {
            return null;
        }

        UserVO userVo = new UserVO();

        return userVo;
    }
}
```

<br/>

**错误原因**：问题其实很简单，`MapStruct`是直接依赖于实体类的 **Getter**、 **Setter**  方法来实现属性值的设置的，因此只需要给两个相互转换的实体类都加上对应的方法即可，上面的由于 UserPO 类已经使用了 lombok 的 @Data 注解，我们只需要给 UserVO 加上对应的 lombok 的 @Data 注解即可。

<br/>

处理完之后再看一次对应的实现类，可以发现实现类代码是正确的了，如下：

```java
package com.xxx;

import javax.annotation.Generated;

@Generated(
    value = "org.mapstruct.ap.MappingProcessor",
    date = "2024-06-28T15:24:13+0800",
    comments = "version: 1.5.0.Beta1, compiler: javac, environment: Java 1.8.0_311 (Oracle Corporation)"
)
public class UserConvertImpl implements UserConvert {

    @Override
    public UserVO convertUserPo2UserVO(UserPO userPo) {
        if ( userPo == null ) {
            return null;
        }

        UserVO userVo = new UserVO();

        userVo.setId( userPo.getId() );
        userVo.setMonitorTime( userPo.getMonitorTime() );
        userVo.setCreateTime( userPo.getCreateTime() );
        userVo.setUpdateTime( userPo.getUpdateTime() );
        userVo.setFlag( userPo.getFlag() );
        return userVo;
    }
}
```









<br/>

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/WorkExperience&Skills/image/Java%E5%BC%80%E5%8F%91-%E5%AE%9E%E9%99%85%E5%B7%A5%E4%BD%9C%E7%BB%8F%E9%AA%8C%E5%92%8C%E6%8A%80%E5%B7%A7-0005-%E4%BD%BF%E7%94%A8MapStruct%E8%BF%9B%E8%A1%8C%E4%B8%A4%E4%B8%AA%E5%AE%9E%E4%BD%93%E7%B1%BB%E7%9A%84%E8%BD%AC%E6%8D%A2,%E5%87%BA%E7%8E%B0%E6%89%80%E6%9C%89%E5%B1%9E%E6%80%A7%E5%80%BC%E9%83%BD%E4%B8%BAnull%E7%9A%84%E6%83%85%E5%86%B5.png?raw=true)

上图由 Pic 生成

关键词：The Pokémon monster that combines three Pokémon: the Little Fire Dragon, the Water Arrow Turtle, and the Bulbasaur Seed

<br/>

**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**