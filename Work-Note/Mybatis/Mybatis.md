[TOC]



# Mybatis





## 1.INSERT

### （1）insert返回主键值

#### a、使用JDBC方式返回主键自增的值

```xml
<insert id="insert2" useGeneratedKeys="true" keyProperty="id">
insert into sys user(user name，user password, user email,user info，head img, create time)
values(
#{userNamel， #{userPassword}, #{userEmail},
#{userInfo}，#{headImg， jdbcType=BLOB},
#{createTime，jdbcType= TIMESTAMP})
</insert>
```

useGeneratedKeys 设置为true后,MyBatis 会使用JDBC的getGeneratedKeys 方法来取出由数据库内部生成的主键。获得主键值后将其赋值给 keyProperty配置的 id 属性。当需要设置多个属性时，使用逗号隔开，这种情况下通常还需要设置 keyColumn 属性，按顺序指定数据库的列，这里列的值会和 keyProperty配置的属性一一对应。由于要使用数据库返回的主键值，所以SQL上下两部分的列中去掉了 id 列和对应的#{id)属性。



```java
@Insert({"insert into sys role(role_name,enabled,create_by, create_time)",
"values(#{roleName}， #{enabled}，{createBy}，",
"#{createTime，jdbcType=TIMESTAMP})"})
@Options(useGeneratedKeys = true, keyProperty ="id")
int insert2(SysRole sysRole);
```

和上面的 insert 方法相比，insert2 方法中的 SOL 中少了id 一列，注解多了一个我们在这个注解中设置了useGeneratedKeys 和 keyProperty 属性，用法和@Options，XML相同，当需要配置多个列时，这个注解也提供了 keyColumn 属性，可以像XML中那样配置使用。



#### b、使用selectKey返回主键的值

上面这种回写主键的方法只适用于支持主键自增的数据库。有些数据库(如 Oracle)不提供主键自增的功能，而是使用序列得到一个值，然后将这个值赋给 id，再将数据插入数据库。对于这种情况，可以采用另外一种方式: 使用<selectKey>标签来获取主键的值，这种方式不仅适用于不提供主键自增功能的数据库，也适用于提供主键自增功能的数据库。

```xml
<!-- MySQL-->
<insert id="insert3">
insert into sys user(user password, user email,user_name，user_info，head_img, create_time)
    values
    (#{userName}， #{userPassword}， #{userEmail},#{headImg，jdbcType=BLOB},#luserInfol#{createTime，jdbcType= TIMESTAMP})
    
    <selectKey keyColumn="id" resultType="long" keyProperty="id" order="AFTER">
        SELECT LAST_INSERT_ID()
	</selectKey>
</insert>

<!--
selectKey标签的 keyColumn、keyProperty和上面useGeneratedKeys 的用法含义相同，这里的 resultType 用于设置返回值类型。order 属性的设置和使用的数据库有关。在MySOL数据库中，order 属性设置的值是AFTER，因为当前记录的主键值在 insert 语执行成功后才能获取到。而在 Oracle 数据库中order 的值要设置为 BEFORE,这是因为Oracle中需要先从序列获取值，然后将值作为主键插入到数据库中。
-->



<!-- Oralce-->
<insert id="insert3">
    
<selectKey keyColumn="id" resultType="long" keyProperty="id"order="BEFORE">
    SELECT SEO_ID.nextval from dual
</selectKey>
    
insert into sys user(id，user_name，user_password,user_email,user_info，head_img，create_time)
    values(#{id}，#{userName]， #{userPassword}，#{userEmail}
    #{userInfo)，#{headImg，jdbcType=BLOB)，#{createTime，jdbcType=TIMESTAMP))
</insert>
<!-- 
可以发现，selectKey 元素放置的位置和之前 MySOL 例子中的不同，其实这个元素放置的位置不会影响 selectKey 中的方法在 insert 前面或者后面执行的顺序，影响执行顺序的是 order 属性，这么写仅仅是为了符合实际的执行顺序，看起来更直观而已。

-->

<!-- 
注意!
Oracle 方式的INSERT 语句中明确写出了id列和值#{id}，因为执行 selectKey 中的语句后 id 就有值了，我们需要把这个序列值作为主键值插入到数据库中，所以必须指定 id列，如果不指定这一列，数据库就会因为主键不能为空而抛出异常
-->
```



```java
@Insert({"insert into sys_role(role_name, enabled, create_by, create_time)",
         "values(#{roleName}， #{enabled}，#{createBy}，",
         "#{createTime，jdbcType= TIMESTAMP})"})
@SelectKey(statement ="SELECT LAST_INSERT_ID()", 
           keyProperty="id",
		  resultType = Long.class,
           before = false)
int insert3(SysRole sysRole);
```

来对比一下，配置属性基本上都是相同的，其中 before 为 false 时功能等同于order="AFTER"，before为true 时功能等同于order="BEFORE"在不同的数据库中，order 的配置不同，大家可以参考前面XML 中的内容，自行编写测试代码进行测试。







## 2.SELECT

### （1）模糊查询

* bind用法

使用 concat 函数连接字符串，在 MySQL中，这个函数支持多个参数，但在 Oracle 中只支持两个参数。由于不同数据库之间的语法差异，如果更换数据库，有些 SOL 语句可能就需要重写。针对这种情况，可以使用 bind 标签来避免由于更换数据库带来的一些麻烦。将上面的方法改为 bind方式后，代码如下。

```xml
<if test="userName != null and userName != ''">
          <bind name="userNameLike" value="'%' + userName + '%'"/>
          and user_name like #{userNameLike}
</if>                                                                          
```

bind 标签的两个属性都是必选项，name 为绑定到上下文的变量名，value 为OGNL表达式。创建一个 bind 标签的变量后，就可以在下面直接使用，使用 bind 拼接字符串不仅可以避免因更换数据库而修改 SOL，也能预防 SOL 注入。大家可以根据需求，灵活使用 OGNI表达式来实现功能.













## Mybatis Generator 配置相关













# --------------------------------------------------

# Mybatus Plus

## 1.INSERT



## 2.SELECT

### （1）Mybatis Plus 特有关联 Mybatis 的动态 SQL 的写法

```xml
<!-- Page<YourObject> listByCondition(Wrapper ew, Page page); -->


<select id="listByCondition" resultType="YourObject">
    SELECT
    <include refid="Base_Column_List"/>
    FROM table_name ${ew.customSqlSegment}
</select>
```

























































