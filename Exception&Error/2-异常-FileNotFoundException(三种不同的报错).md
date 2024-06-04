# 2-异常-FileNotFoundException(三种不同的报错)



**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**



**FileNotFoundException**：文件找不到异常，FileNotFoundException出现有几种可能性，可参考下列内容。

* FileNotFoundException: D:\XXX\XXX.xlsx
* FileNotFoundException: D:\XXX\XXX\XXX.xlsx (另一个程序正在使用此文件，进程无法访问。)
* FileNotFoundException: D:\XXX\XXX\XXX.xlsx (拒绝访问。)





## 报错一： FileNotFoundException: D:\XXX\XXX.xlsx

```txt
cn.hutool.poi.exceptions.POIException: FileNotFoundException: D:\XXX\XXX.xlsx


Caused by: java.io.FileNotFoundException: D:\XXX\XXX.xlsx
	at org.apache.poi.ss.usermodel.WorkbookFactory.create(WorkbookFactory.java:317)
	at org.apache.poi.ss.usermodel.WorkbookFactory.create(WorkbookFactory.java:295)
	at cn.hutool.poi.excel.WorkbookUtil.createBook(WorkbookUtil.java:84)
	... 38 more
```

可能的原因：

* FileNotFoundException字面意思**读取的文件路径对应的文件不存在**
* 读取的**文件路径可能写错**





## 报错二：FileNotFoundException: D:\XXX\XXX\XXX.xlsx (另一个程序正在使用此文件，进程无法访问。)

```txt

cn.hutool.poi.exceptions.POIException: FileNotFoundException: D:\XXX\XXX\XXX.xlsx (另一个程序正在使用此文件，进程无法访问。)


Caused by: java.io.FileNotFoundException: D:\XXX\XXX\XXX.xlsx (另一个程序正在使用此文件，进程无法访问。)
	at java.io.RandomAccessFile.open0(Native Method)
	at java.io.RandomAccessFile.open(RandomAccessFile.java:316)
	at java.io.RandomAccessFile.<init>(RandomAccessFile.java:243)
	at org.apache.poi.poifs.nio.FileBackedDataSource.newSrcFile(FileBackedDataSource.java:158)
	at org.apache.poi.poifs.nio.FileBackedDataSource.<init>(FileBackedDataSource.java:60)
	at org.apache.poi.poifs.filesystem.POIFSFileSystem.<init>(POIFSFileSystem.java:217)
	at org.apache.poi.poifs.filesystem.POIFSFileSystem.<init>(POIFSFileSystem.java:170)
	at org.apache.poi.ss.usermodel.WorkbookFactory.create(WorkbookFactory.java:322)
	at org.apache.poi.ss.usermodel.WorkbookFactory.create(WorkbookFactory.java:295)
	at cn.hutool.poi.excel.WorkbookUtil.createBook(WorkbookUtil.java:84)
	... 38 more

```

可能的原因:

* 报错内容里面直接说明了，**读取的文件正在被另一个程序使用，导致无法访问，本地将打开这个文件的程序关闭即可**。







## 报错三： FileNotFoundException: D:\XXX\XXX\XXX.xlsx (拒绝访问。)

```txt
cn.hutool.poi.exceptions.POIException: FileNotFoundException: D:\XXX\XXX\XXX.xlsx (拒绝访问。)


Caused by: java.io.FileNotFoundException: D:\XXX\XXX\XXX.xlsx (拒绝访问。)
	at java.io.RandomAccessFile.open0(Native Method)
	at java.io.RandomAccessFile.open(RandomAccessFile.java:316)
	at java.io.RandomAccessFile.<init>(RandomAccessFile.java:243)
	at org.apache.poi.poifs.nio.FileBackedDataSource.newSrcFile(FileBackedDataSource.java:158)
	at org.apache.poi.poifs.nio.FileBackedDataSource.<init>(FileBackedDataSource.java:60)
	at org.apache.poi.poifs.filesystem.POIFSFileSystem.<init>(POIFSFileSystem.java:217)
	at org.apache.poi.poifs.filesystem.POIFSFileSystem.<init>(POIFSFileSystem.java:170)
	at org.apache.poi.ss.usermodel.WorkbookFactory.create(WorkbookFactory.java:322)
	at org.apache.poi.ss.usermodel.WorkbookFactory.create(WorkbookFactory.java:295)
	at cn.hutool.poi.excel.WorkbookUtil.createBook(WorkbookUtil.java:84)
	... 38 more


```

可能的原因：

* 读取的文件路径的文件属性被设置成了**只读**，把**文件对应的属性的只读状态移除即可**。（Windows电脑：(鼠标在文件位置)右键 -> 属性 -> 属性列的**只读**勾选去掉 -> 应用）





**更多内容欢迎关注我（持续更新中，欢迎Star✨）**

**Github：[CodeZeng1998/Java-Developer-Work-Note](https://github.com/CodeZeng1998/Java-Developer-Work-Note)**

**技术公众号：CodeZeng1998**（纯纯技术文）

**生活公众号：好锅**（Life is more than code）

**CSDN: CodeZeng1998**

**其他平台：CodeZeng1998**、**好锅**