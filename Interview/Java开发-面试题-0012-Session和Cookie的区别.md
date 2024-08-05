# Java开发-面试题-0012-Session和Cookie的区别



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**







==Session 和 Cookie 的区别==

**存储位置**

* Cookie: ==存储在客户端（通常是用户的浏览器）==。
* Session: ==存储在服务器端==。



**安全性**

* Cookie: 不是很安全，因为数据存储在客户端，容易被恶意用户截取或篡改。
* Session: 相对更安全，因为数据存储在服务器端，客户端只能通过会话ID（通常通过 cookie 传输）来访问。



**性能**

* Cookie: 每次HTTP请求都会携带 cookie 数据，可能会增加网络负载。
* Session: 只需要传输会话ID，数据存储在服务器端，可以减少网络流量。



**生命周期**

* Cookie: 可以设置过期时间，可以在浏览器关闭后仍然存在。
* Session: 通常在浏览器关闭后失效，但也可以配置持久化。



**数据大小限制**

* Cookie: 单个 cookie 的数据大小有限制（通常为4KB）。
* Session: 由于存储在服务器端，数据大小限制取决于服务器配置。





**扩展**

**关闭 Cookie 对 Session 的影响**

* 当客户端禁用了 cookie 功能时，服务器无法通过 cookie 向客户端发送会话ID。这意味着传统的基于 cookie 的 session 机制将无法正常工作。
* 但是，如果服务器实现了一些备选机制来传递会话ID，比如将会话ID嵌入URL中或者作为HTTP请求参数传递，那么即使没有 cookie 支持，仍然可以维持会话状态。
* 为了确保兼容性和安全性，通常的做法是建议用户启用 cookie，并在禁用 cookie 的情况下提供降级处理或提示用户重新启用 cookie。



总的来说，session 和 cookie 都是用于维护Web应用中的会话状态，但它们分别存储在服务器端和客户端。关闭 cookie 通常会导致基于 cookie 的 session 机制失效，除非采取其他措施来传递会话ID。





以上就是本文相关的所有内容了，如果发现有误欢迎评论指正，更多内容欢迎各位关注。

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Interview/image/0012.png?raw=true)

上图是由 Pic 生成的

关键词：Cyber style programmer

<br/>

**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**

