# Java开发-面试题-0019-static 关键字平时用过吗，怎么用的，有什么好处，原理是什么



> **更多内容欢迎关注我（持续更新中，欢迎Star✨）**
>
> **Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**
>
> **（技术）微信公众号：CodeZeng1998**
>
> **（生活）微信公众号：好锅**
>
> **其他平台：CodeZeng1998**、**好锅**



static 关键字平时用过吗，怎么用的，有什么好处，原理是什么



### static 修饰范围

* 变量
* 方法
* 代码快
* 内部类



### 用法

1. **静态变量（类变量）**: 用`static`修饰的变量==属于类而不是实例，因此所有实例共享同一个变量==。例如：

   ```java
   public class Example {
       static int count = 0;
   }
   ```

   所有`Example`对象共享同一个`count`变量。

2. **静态方法**: 用`static`修饰的方法属于类而不是对象，因此可以在不创建实例的情况下调用。例如：

   ```java
   public class Example {
       static void printHello() {
           System.out.println("Hello, World!");
       }
   }
   ```

   可以直接使用`Example.printHello()`调用方法。

3. **静态代码块**: 在类加载时运行，只执行一次，通常用于初始化静态变量。例如：

   ```java
   public class Example {
       static int count;
       static {
           count = 10;
       }
   }
   ```

4. **静态内部类**: 静态内部类可以不依赖外部类的实例，直接通过类名访问。例如：

   ```java
   public class Outer {
       static class Inner {
           void display() {
               System.out.println("Static Inner Class");
           }
       }
   }
   ```

   可以直接通过`Outer.Inner`访问内部类。

### 好处

1. **节省内存**: ==静态成员在内存中只存储一份，适合共享的场景==。
2. **简化调用**: 静态方法可以直接通过类名调用，不需要创建实例。
3. **初始化顺序**: 静态代码块可以用于控制复杂初始化逻辑。

### 原理

==`static`关键字影响类的加载过程。静态变量和方法在类加载时就被分配内存，存在于方法区（Method Area）中，不随对象的创建或销毁而变化。这意味着无论创建多少个对象，静态成员都只存在一份，且可以通过类名直接访问。==





以上就是本文相关的所有内容了，如果发现有误欢迎评论指正，更多内容欢迎各位关注。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0019.png?raw=true)

上图是由 Pic 生成的

关键词：The water lord Pok é mon in Pok é mon runs on the water surface

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

