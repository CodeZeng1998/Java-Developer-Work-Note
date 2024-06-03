# Java开发-面试题-0005-==和String的equals()和String的intern()方法的区别



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**







**==和String的equals()和String的intern()方法的区别:**



* **==** 

  * 相比较的数据都是**基本数据类型**，比较的是**数值**。
  * 相比较的数据都是**引用数据类型（对象）**，比较的是两个引用是否指向内存中的同一个对象。

* java.lang.**String#equals**

  * `equals()` 方法用于比较两个对象的内容或值。

  * 默认情况下，`Object`类中的`equals()`方法的行为类似于`==`（检查引用是否相等），但许多类（如`String`，`Integer`等）重写此方法以提供基于值的比较。

    ```java
        // String 的 equals 方法
    	public boolean equals(Object anObject) {
            if (this == anObject) {
                return true;
            }
            if (anObject instanceof String) {
                String anotherString = (String)anObject;
                int n = value.length;
                if (n == anotherString.value.length) {
                    char v1[] = value;
                    char v2[] = anotherString.value;
                    int i = 0;
                    while (n-- != 0) {
                        if (v1[i] != v2[i])
                            return false;
                        i++;
                    }
                    return true;
                }
            }
            return false;
        }
    
    
    	// Integer 的 equals 方法
        public boolean equals(Object obj) {
            if (obj instanceof Integer) {
                return value == ((Integer)obj).intValue();
            }
            return false;
        }
    ```

* java.lang.String#intern

  * `intern()` 方法用于通过使用字符串池有效地管理内存。
  * **当在一个 `String` 对象上调用 `intern()` 方法时，它检查字符串是否已经在字符串池中。如果是，它返回池中的引用。如果不是，它将字符串添加到池中并返回新添加字符串的引用。**
  * 这可以使得对具有相同内容且已被实习的字符串的后续 `==` 比较为真。

```java
String str1 = new String("hello");
String str2 = new String("hello");

String str3 = str1.intern();
String str4 = str2.intern();

System.out.println(str1 == str2); // false
System.out.println(str1.equals(str2)); // true
System.out.println(str3 == str4); // true
```







![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0005.jpeg?raw=true)

上图由 RealVisXL 生成

关键词：An alien wearing sunglasses , poses a Java question







**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**

