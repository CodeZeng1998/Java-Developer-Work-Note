# Java开发-实际工作经验和技巧-0003-容易被忽视的Git提交代码规范

**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**

<br/>



本文分享一个容易被大家忽视的 Git 里面的规范相关的内容，那就是**提交信息的编写规范**。



好的 Git 的提交信息，要满足几个条件：

* **简单易懂**
* **容易区分**
* **一次提交尽可能只跟一个功能相关**



下面介绍详细的内容，大家可以按照这个规范来进行 Git 提交信息的编写（**推荐**），或者按照公司自定义规范进行相应的提交信息的编写。

<br/>

提交信息的**格式**如下：

```txt
<type>(<scope>): <subject>

<body>

BREAKING CHANGE: <changes>

Closes <closes>

<skipCi>
```



* type ：提交类型
* scope：范围
* subject：简短描述
* body：详细描述

<br/>

In file templates, you can use text, code, comments, and predefined variables. The following provides a list of predefined variables. 

 Predefined variables take the following values: 

| 标识       | 含义                                                      |
| ---------- | --------------------------------------------------------- |
| ${type}    | Corresponds to the submission menu 'Type of change'       |
| ${scope}   | Corresponds to the submission menu 'Scope of this change' |
| ${subject} | Corresponds to the submission menu 'Short description'    |
| ${body}    | Corresponds to the submission menu 'Long description'     |
| ${changes} | Corresponds to the submission menu 'Breaking changes'     |
| ${closes}  | Corresponds to the submission menu 'Closed issues'        |
| ${skipCi}  | Corresponds to the submission menu 'Skip CI'              |
| ${newLine} | ' ', indicating whether to wrap the line                  |

<br/>

**最常用**的其实是如下模板：即 **类型(范围): 简短信息**

```txt
<type>(<scope>): <subject>
```





下面介绍一下 **type** 的详细使用：

| 类型种类 | 含义                                                         |
| -------- | ------------------------------------------------------------ |
| feat     | A new feature                                                |
| fix      | A bug fix                                                    |
| docs     | Documentation only changes                                   |
| style    | Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc) |
| refactor | A code change that neither fixes a bug nor adds a feature    |
| perf     | A code change that improves performance                      |
| test     | Adding missing tests or correcting existing tests            |
| build    | Changes that affect the build system or external dependencies (example scopes: gulp, broccoli, npm) |
| ci       | Changes to our CI configuration files and scripts (example scopes: Travis, Circle, BrowserStack, SauceLabs) |
| chore    | Other changes that don't modify src or test files            |
| revert   | Reverts a previous commit                                    |

常用的 **type** 主要有以下几个：

* type：新的特性，新的功能点
* fix： 修复一个 bug
* docs：只有文档相关内容变更
* style：代码风格变更（代码格式、空格、空行等）
* perf ：性能优化
* test：测试相关内容



如果使用的开发工具是 **IDEA**，推荐使用插件 **Git Commit Message Helper**，习惯之后就可以自己按照上述格式进行 Git 提交信息的编写即可。



下面给出几个符合上述规范的 Git 提交信息的例子：

* feat(用户模块): 新增用户列表查询功能；
* fix(用户模块): 修改用户列表查询分页错误bug；
* docs(用户模块): 增加对应的用户角色文档说明；
* style(用户模块): 更改代码风格；
* perf(用户模块):  引入Reids缓存对用户列表查询功能进行优化；
* test用户模块):  增加用户列表查询功能的单元测试；





**建议：**

* **Git 的提交信息按照上述规范进行编写**
* **一次性提交的内容尽可能的少，并且不相关的内容分开进行提交**





<br/>

![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/WorkExperience&Skills/image/Java%E5%BC%80%E5%8F%91-%E5%AE%9E%E9%99%85%E5%B7%A5%E4%BD%9C%E7%BB%8F%E9%AA%8C%E5%92%8C%E6%8A%80%E5%B7%A7-0003-%E5%AE%B9%E6%98%93%E8%A2%AB%E5%BF%BD%E8%A7%86%E7%9A%84Git%E6%8F%90%E4%BA%A4%E4%BB%A3%E7%A0%81%E8%A7%84%E8%8C%83.png?raw=true)

上图由 Pic 生成

关键词：A bustling market street in an ancient city

<br/>

**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**