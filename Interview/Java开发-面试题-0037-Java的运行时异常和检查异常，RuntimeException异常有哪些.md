# Java开发-面试题-0037-Java的运行时异常和检查异常，RuntimeException异常有哪些

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

> PS：这个异常具体的内容还是需要知道的，之前面试的时候突然让我举多个运行时异常的例子，说出来的数量有点不够，所以还是需要稍微注意一下这块内容，当然只需要记住常见的几个就可以了。

<br/>

在 Java 中，异常分为两大类：**运行时异常**（也称为**非检查异常**）和**检查异常**。

<br/>

### 1. 运行时异常（RuntimeException）

**运行时异常**是继承自 `RuntimeException` 的异常，它们是非检查异常，即编译时不会强制要求开发者处理这些异常。这类异常通常是由于编程错误引发的，比如空指针引用、数组越界等。开发者可以选择捕获这些异常，也可以不处理，系统会在运行时抛出这些异常。

<br/>

**特点**：

- ==**非强制处理**：编译器不会强制要求捕获或声明这些异常。==
- **通常是编程错误**：比如调用方法时传递了不合法的参数。
- **可以不捕获**：如果没有处理，运行时会终止程序并抛出异常。

<br/>

**常见的运行时异常包括**：

- **`NullPointerException`**：当尝试访问一个未初始化（`null`）的对象时抛出。

- **`ArrayIndexOutOfBoundsException`**：当访问数组时，索引超出数组的有效范围时抛出。

- **`ArithmeticException`**：通常在算术运算异常时抛出，如除以零。

- **`ClassCastException`**：当试图将对象强制转换为不是其实例的类型时抛出。

- `IllegalArgumentException`

  ：当向方法传递了非法或不适合的参数时抛出。

  - **`NumberFormatException`**：`IllegalArgumentException` 的子类，当试图将一个字符串转换为数值类型时，字符串的格式不合法时抛出。

- **`IllegalStateException`**：当方法在不适合的时间调用时抛出，比如对象状态不满足操作的前置条件。

- `IndexOutOfBoundsException`：当访问列表或数组时，索引越界时抛出。

  - **`ArrayIndexOutOfBoundsException`**：`IndexOutOfBoundsException` 的子类，用于数组越界。
  - **`StringIndexOutOfBoundsException`**：当操作字符串时，索引超出范围时抛出。

- **`UnsupportedOperationException`**：当调用的操作不被支持时抛出。

- **`ConcurrentModificationException`**：当检测到对象的并发修改（通常是集合）但不允许并发修改时抛出。

- **`NegativeArraySizeException`**：当试图创建一个大小为负数的数组时抛出。

- **`SecurityException`**：与安全性相关的异常，比如访问控制、权限问题。

- **`MissingResourceException`**：当找不到所需的资源（如国际化时找不到指定的资源）时抛出。

- **`IllegalMonitorStateException`**：当线程试图在它没有持有的对象监视器上等待或通知时抛出。

- **`EnumConstantNotPresentException`**：当枚举类中使用了不存在的枚举常量时抛出。

- **`TypeNotPresentException`**：当类型加载失败时抛出。

<br/>

### 2. 检查异常（Checked Exception）

**检查异常**是继承自 `Exception` 但不继承自 `RuntimeException` 的异常。这类异常在编译时必须要处理，开发者需要显式捕获或声明这些异常，否则编译不会通过。检查异常通常是由外部因素导致的异常，如文件未找到、网络连接失败等。

**特点**：

- ==**必须处理**：编译时编译器强制要求捕获或在方法签名中声明异常。==
- **外部错误**：通常由外部环境因素或系统错误引发，如 I/O 操作失败、网络问题等。
- **典型例子**：`IOException`、`SQLException`、`FileNotFoundException`、`ClassNotFoundException` 等。

<br/>

### 3. 常见的 `RuntimeException` 子类说明

下面列出了 Java 中常见的 `RuntimeException` 的具体情况：

#### 1. `NullPointerException`

- **原因**：当尝试对 `null` 对象调用方法或访问属性时抛出。
- **解决方案**：确保对象在使用前被正确初始化，并且在使用前进行空值检查。

#### 2. `ArrayIndexOutOfBoundsException`

- **原因**：当访问数组时，索引超出数组的合法范围时抛出。
- **解决方案**：确保索引在数组的合法范围内（`0 <= index < array.length`）。

#### 3. `ArithmeticException`

- **原因**：通常在算术运算异常时抛出，例如除以零。
- **解决方案**：确保在进行运算之前检查分母是否为零。

#### 4. `ClassCastException`

- **原因**：当对象被强制转换为与其实际类型不兼容的类时抛出。
- **解决方案**：在进行强制转换前使用 `instanceof` 检查对象的实际类型。

#### 5. `IllegalArgumentException`

- **原因**：向方法传递了非法的参数时抛出。
- **解决方案**：在方法调用前验证输入参数是否合法。

#### 6. `IllegalStateException`

- **原因**：当对象的状态不允许调用某个方法时抛出，例如在对象未初始化前调用某个需要初始化的操作。
- **解决方案**：在调用方法之前检查对象的状态是否允许执行该操作。

#### 7. ==`ConcurrentModificationException`==

- **原因**：当对集合进行遍历时，同时修改集合（如添加、删除元素）时抛出。
- **解决方案**：在遍历集合时不要修改集合，或者使用并发集合类（如 `CopyOnWriteArrayList`）。

<br/>

### 4. 运行时异常的处理建议

虽然 `RuntimeException` 并不强制要求捕获和处理，但为了提高代码的鲁棒性，开发者仍然可以根据具体场景采取适当的处理措施：

- **空指针检查**：对于可能为 `null` 的对象，进行非空检查。
- **输入验证**：在使用参数之前，验证其合法性。
- **边界检查**：在使用数组或列表时，确保索引在合法范围内。
- **使用容器类的 `Optional`**：在可能存在空值的情况下，使用 `Optional` 避免 `NullPointerException`。
- **日志记录**：在捕获异常时，及时记录异常信息，便于后续调试和维护。

<br/>

### 5. 总结

**运行时异常**（`RuntimeException`）在 Java 中虽然不要求开发者显式处理，但在实际开发中仍需关注这些异常的处理，尤其是在核心业务逻辑中，以避免程序在运行时崩溃。常见的运行时异常包括 `NullPointerException`、`ArrayIndexOutOfBoundsException`、`ArithmeticException` 等。适当的输入验证和异常捕获机制能够极大地提高程序的健壮性和可维护性。





<br/>

以上就是本文相关的所有内容了，如果发现有误欢迎评论指正，更多内容欢迎各位关注。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0037.png?raw=true)

上图是由 Pic 生成的

关键词：The Avengers

<br/>



> **更多内容欢迎关注我（持续更新中，欢迎Star✨）**
>
> **Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**
>
> **（技术）微信公众号：CodeZeng1998**
>
> **（生活）微信公众号：好锅**
>
> **其他平台：CodeZeng1998**、**好锅**



