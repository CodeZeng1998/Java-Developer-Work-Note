# Java开发-面试题-0001-String、StringBuilder、StringBuffer的区别





**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**





> 说明：Java 版本为 1.8





String、StringBuilder、StringBuffer的区别

* **可变性**

  * String：不可改变，一旦创建就无法修改。（扩展：字符串常量池~）

  * StringBuilder：可变的，内容可修改。

  * StringBuffer：可变的，内容可修改。

    

* **线程安全性**

  * String：不可变的，天生线程安全。

    ```java
    // 底层是字符数组
    private final char value[];
    ```

  * StringBuilder：非线程安全。

  * StringBuffer：线程安全。（synchronized修饰）

    ```java
        @Override
        public synchronized StringBuffer append(Object obj) {
            toStringCache = null;
            super.append(String.valueOf(obj));
            return this;
        }
    ```

    

* **性能**

  * String：由于不可变性，对于频繁修改来说效率较低。
  * StringBuilder：由于可变性，对于频繁修改来说效率较高。
  * StringBuffer：由于线程安全的开销，略低于StringBuilder。

* **内存使用**

  * String：可能导致内存浪费，因为修改会创建新对象。（每当执行任何看似修改字符串的操作，如连接（`+` 操作符）、创建子字符串或替换时，都会在内存中创建一个新的字符串对象，其内容被修改。）

  * StringBuilder：内存使用更高效，因为可以直接修改内容而不创建新对象。

    ```java
    // StringBuilder 可以在不创建新的 String 对象的情况下，直接修改其内部的字符数组，并实时更新内容。这种直接的字符数组操作使得在进行字符串追加时能够避免额外的内存分配和对象创建，从而提高了性能和内存使用效率。
        public AbstractStringBuilder append(String str) {
            if (str == null)
                return appendNull();
            int len = str.length();
            // 数组扩容
            ensureCapacityInternal(count + len);
            // 追加字符串，底层是 native 修饰的System.arraycopy(...)方法实现数组拷贝
            str.getChars(0, len, value, count);
            count += len;
            return this;
        }
    ```

  * StringBuffer：与StringBuilder类似，但由于线程安全的原因略有开销。

* **使用场景**

  * String：适用于需要不可变性和线程安全性，并且修改较少的场景。
  * StringBuilder：适用于需要频繁修改而不需要线程安全的场景。
  * StringBuffer：适用于需要线程安全性的场景，即需要牺牲一些性能。





**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**

