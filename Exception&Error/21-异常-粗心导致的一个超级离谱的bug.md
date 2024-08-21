# 21-异常-粗心导致的一个超级离谱的bug



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**



<br/>



**问题描述**：项目里面使用 IDEA 里的插件 MybatisX-Generator 对某张表进行基础的 MVC 架构代码生成时，出现了报错（如下），找了很久一直没发觉原因最终才排查到是==表名的问题==，各位后续如果遇见可以多注意一点。自动生成代码时只生成了对应的实体类。（其实这里的实体类代码里就可以看出表名有问题了），但由于一直没往这方面想导致排查浪费了一点时间。



<br/>

**报错信息：**

```txt
生成代码出错

java.io.FileNotFoundException: D:\xxx\project\code\xxxApp\xxx-common\src\main\java\generator\service\impl\XxxtStatistics
ServiceImpl.java (文件名、目录名或卷标语法不正确。)
	at java.base/java.io.FileOutputStream.open0(Native Method)
	at java.base/java.io.FileOutputStream.open(FileOutputStream.java:293)
	at java.base/java.io.FileOutputStream.<init>(FileOutputStream.java:235)
	at com.baomidou.plugin.idea.mybatisx.generate.plugin.IntellijMyBatisGenerator.writeFile(IntellijMyBatisGenerator.java:313)
	at com.baomidou.plugin.idea.mybatisx.generate.plugin.IntellijMyBatisGenerator.writeGeneratedJavaFile(IntellijMyBatisGenerator.java:249)
	at com.baomidou.plugin.idea.mybatisx.generate.plugin.IntellijMyBatisGenerator.generate(IntellijMyBatisGenerator.java:159)
	at com.baomidou.plugin.idea.mybatisx.generate.template.GenerateCode.generate(GenerateCode.java:163)
	at com.baomidou.plugin.idea.mybatisx.generate.action.MybatisGeneratorMainAction.generateCode(MybatisGeneratorMainAction.java:85)
	at com.baomidou.plugin.idea.mybatisx.generate.action.MybatisGeneratorMainAction.actionPerformed(MybatisGeneratorMainAction.java:65)
	at com.intellij.openapi.actionSystem.ex.ActionUtil.doPerformActionOrShowPopup(ActionUtil.java:333)
	at com.intellij.openapi.actionSystem.ex.ActionUtil.lambda$performActionDumbAwareWithCallbacks$4(ActionUtil.java:307)
	at com.intellij.openapi.actionSystem.ex.ActionUtil.performDumbAwareWithCallbacks(ActionUtil.java:356)
	at com.intellij.openapi.actionSystem.ex.ActionUtil.performActionDumbAwareWithCallbacks(ActionUtil.java:307)
	at com.intellij.openapi.actionSystem.impl.ActionMenuItem.lambda$performAction$5(ActionMenuItem.java:299)
	at com.intellij.openapi.wm.impl.FocusManagerImpl.runOnOwnContext(FocusManagerImpl.java:225)
	at com.intellij.openapi.actionSystem.impl.ActionMenuItem.performAction(ActionMenuItem.java:292)
	at com.intellij.openapi.actionSystem.impl.ActionMenuItem.lambda$new$0(ActionMenuItem.java:67)
	at java.desktop/javax.swing.AbstractButton.fireActionPerformed(AbstractButton.java:1972)
	at com.intellij.openapi.actionSystem.impl.ActionMenuItem.lambda$fireActionPerformed$4(ActionMenuItem.java:114)
	at com.intellij.openapi.application.TransactionGuardImpl.performActivity(TransactionGuardImpl.java:105)
	at com.intellij.openapi.application.TransactionGuardImpl.performUserActivity(TransactionGuardImpl.java:94)
	at com.intellij.openapi.actionSystem.impl.ActionMenuItem.fireActionPerformed(ActionMenuItem.java:114)
	at com.intellij.ui.plaf.beg.BegMenuItemUI.doClick(BegMenuItemUI.java:526)
	at com.intellij.ui.plaf.beg.BegMenuItemUI$MyMouseInputHandler.mouseReleased(BegMenuItemUI.java:558)
	at java.desktop/java.awt.Component.processMouseEvent(Component.java:6656)
	at java.desktop/javax.swing.JComponent.processMouseEvent(JComponent.java:3385)
	at java.desktop/java.awt.Component.processEvent(Component.java:6421)
	at java.desktop/java.awt.Container.processEvent(Container.java:2266)
	at java.desktop/java.awt.Component.dispatchEventImpl(Component.java:5026)
	at java.desktop/java.awt.Container.dispatchEventImpl(Container.java:2324)
	at java.desktop/java.awt.Component.dispatchEvent(Component.java:4854)
	at java.desktop/java.awt.LightweightDispatcher.retargetMouseEvent(Container.java:4948)
	at java.desktop/java.awt.LightweightDispatcher.processMouseEvent(Container.java:4575)
	at java.desktop/java.awt.LightweightDispatcher.dispatchEvent(Container.java:4516)
	at java.desktop/java.awt.Container.dispatchEventImpl(Container.java:2310)
	at java.desktop/java.awt.Window.dispatchEventImpl(Window.java:2804)
	at java.desktop/java.awt.Component.dispatchEvent(Component.java:4854)
	at java.desktop/java.awt.EventQueue.dispatchEventImpl(EventQueue.java:790)
	at java.desktop/java.awt.EventQueue$3.run(EventQueue.java:739)
	at java.desktop/java.awt.EventQueue$3.run(EventQueue.java:731)
	at java.base/java.security.AccessController.doPrivileged(AccessController.java:399)
	at java.base/java.security.ProtectionDomain$JavaSecurityAccessImpl.doIntersectionPrivilege(ProtectionDomain.java:86)
	at java.base/java.security.ProtectionDomain$JavaSecurityAccessImpl.doIntersectionPrivilege(ProtectionDomain.java:97)
	at java.desktop/java.awt.EventQueue$4.run(EventQueue.java:763)
	at java.desktop/java.awt.EventQueue$4.run(EventQueue.java:761)
	at java.base/java.security.AccessController.doPrivileged(AccessController.java:399)
	at java.base/java.security.ProtectionDomain$JavaSecurityAccessImpl.doIntersectionPrivilege(ProtectionDomain.java:86)
	at java.desktop/java.awt.EventQueue.dispatchEvent(EventQueue.java:760)
	at com.intellij.ide.IdeEventQueue.defaultDispatchEvent(IdeEventQueue.kt:667)
	at com.intellij.ide.IdeEventQueue.dispatchMouseEvent(IdeEventQueue.kt:615)
	at com.intellij.ide.IdeEventQueue._dispatchEvent(IdeEventQueue.kt:570)
	at com.intellij.ide.IdeEventQueue.access$_dispatchEvent(IdeEventQueue.kt:68)
	at com.intellij.ide.IdeEventQueue$dispatchEvent$processEventRunnable$1$1$1.compute(IdeEventQueue.kt:349)
	at com.intellij.ide.IdeEventQueue$dispatchEvent$processEventRunnable$1$1$1.compute(IdeEventQueue.kt:348)
	at com.intellij.openapi.progress.impl.CoreProgressManager.computePrioritized(CoreProgressManager.java:787)
	at com.intellij.ide.IdeEventQueue$dispatchEvent$processEventRunnable$1$1.invoke(IdeEventQueue.kt:348)
	at com.intellij.ide.IdeEventQueue$dispatchEvent$processEventRunnable$1$1.invoke(IdeEventQueue.kt:343)
	at com.intellij.ide.IdeEventQueueKt.performActivity$lambda$1(IdeEventQueue.kt:995)
	at com.intellij.openapi.application.TransactionGuardImpl.performActivity(TransactionGuardImpl.java:113)
	at com.intellij.ide.IdeEventQueueKt.performActivity(IdeEventQueue.kt:995)
	at com.intellij.ide.IdeEventQueue.dispatchEvent$lambda$4(IdeEventQueue.kt:343)
	at com.intellij.openapi.application.impl.ApplicationImpl.runIntendedWriteActionOnCurrentThread(ApplicationImpl.java:831)
	at com.intellij.ide.IdeEventQueue.dispatchEvent(IdeEventQueue.kt:385)
	at java.desktop/java.awt.EventDispatchThread.pumpOneEventForFilters(EventDispatchThread.java:207)
	at java.desktop/java.awt.EventDispatchThread.pumpEventsForFilter(EventDispatchThread.java:128)
	at java.desktop/java.awt.EventDispatchThread.pumpEventsForHierarchy(EventDispatchThread.java:117)
	at java.desktop/java.awt.EventDispatchThread.pumpEvents(EventDispatchThread.java:113)
	at java.desktop/java.awt.EventDispatchThread.pumpEvents(EventDispatchThread.java:105)
	at java.desktop/java.awt.EventDispatchThread.run(EventDispatchThread.java:92)

```



<br/>

**错误原因**：看了一些文章对其的分析的一些基础报错，一直没往表名不正确的方向去想。



这个错误信息提示在生成代码时，系统无法找到或创建目标文件 `XxxStatisticsServiceImpl.java`。具体错误原因是文件名、目录名或卷标语法不正确。这可能是由于以下几个原因导致的：

1. **文件路径问题**：检查路径 `D:\xxx\project\code\xxxApp\xxx-common\src\main\java\generator\service\impl\XxxStatisticsServiceImpl.java` 是否正确。确保路径中没有不合法的字符，特别是文件名中不能包含冒号、问号、双引号等特殊字符。
2. **路径长度问题**：Windows 系统对路径长度有一定限制，超过限制可能会导致文件无法创建。建议检查路径长度是否超过 260 个字符。
3. **目录权限问题**：确保你的程序有权限在该目录下创建文件。你可以尝试手动在该目录下创建一个文件，验证是否有权限。
4. **目录不存在**：确保路径中的所有目录都存在。如果某个目录不存在，系统将无法创建目标文件。

针对这些原因，建议你进行以下检查和操作：

1. **检查路径**：
   - 确认路径正确无误。
   - 确认路径中没有不合法的字符。
2. **检查路径长度**：
   - 确认路径长度是否超过系统限制，必要时缩短路径。
3. **检查目录权限**：
   - 确保你有权限在目标目录下创建文件。
4. **检查目录存在性**：
   - 确保路径中的所有目录都已经存在，必要时手动创建这些目录。

如果以上检查和操作仍无法解决问题，请提供更多的上下文信息，例如 MybatisX-Generator 的配置文件内容，以便进一步分析和解决问题。



<br/>

**解决方案**：我这边最终找到的问题完全是因为表创建过程中，表名后面不小心回车了一下，导致问题的出现，这个问题其实很好排查，如果能看一下表结构语句或者自己手写 SQL 查询一下即可发现问题。后续如果遇见上述报错，可按照步骤排查，或者检查一下是否有本文所说的问题出现。

<br/>

**建议**：项目里更加建议使用管理工具进行数据库变更的管理，例如我上述的问题如果有管理工具即可直接看出对应的SQL里面的表名出现问题了。不过由于我这里涉及的是一个非常小的项目，所以这种情况的话，建议创建表后，看一下表结构设计，即可看出是否有问题了。





![](https://github.com/CodeZeng1998/Java-Developer-Work-Note/blob/main/Exception&Error/image/21-%E5%BC%82%E5%B8%B8-%E7%B2%97%E5%BF%83%E5%AF%BC%E8%87%B4%E7%9A%84%E4%B8%80%E4%B8%AA%E8%B6%85%E7%BA%A7%E7%A6%BB%E8%B0%B1%E7%9A%84bug.png?raw=true)

上图是由 Pic 生成的

关键词：A monkey rides on an eagle spreading its wings and soaring in the sky

<br/>



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**其他平台：CodeZeng1998**、**好锅**





