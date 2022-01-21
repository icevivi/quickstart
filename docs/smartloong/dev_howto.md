# 如何为智龙IDE开发扩展组件？

[智龙集成开发环境](/smartloong/index)是为使用龙芯1C/1B系列芯片的智龙开发板提供的集成式开发环境（本文中简称智龙IDE）。

关于扩展组件的使用及开发接口等详细信息可以查看 [智龙IDE用户操作手册](https://www.bilive.com/site_media/media/setup/smartloong/help/smartloong_1.0.005_manual.pdf)  。

扩展组件使用的是“biLive跨平台通用应用程序”的格式，扩展名为“PFF”。PFF程序由**biForm**开发和打包，使用Python3做为编程语言，程序中可以使用Qt5库、Python基础库及Python第三方库。

## Python开发接口

开发插件时，如果需要与智龙IDE本身的功能进行集成，需要使用```import IDE```导入IDE模块。

IDE中可以访问“应用程序主窗口”及“串口监视器”等的接口。

开发者可以通过智龙IDE的“控制台”中的“Python命令交互”窗口，通过Python命令先熟悉和了解这些对象及其开放接口。

### 1、主窗口和应用程序

“应用程序主窗口”通过 mainwindow 对象访问，IDE为访问提供了以下接口：

|                                                     方法                                                      |             	说明              |
| ------------------------------------------------------------------------------------------------------------- | ------------------------------ |
| void showMonitor()                                                                                            | 显示监视器窗口                  |
| void hideMonitor()                                                                                            | 关闭监视器窗口                  |
| void showConsole()                                                                                            | 显示控制台窗口                  |
| void hideConsole()                                                                                            | 隐藏控制台窗口                  |
| void createNew()                                                                                              | 新建一个源程序文档              |
| void openFile(const QString& &filename)                                                                       | 打开程序文件                    |
| void closeCurrentSubWindow()	                                                                                | 关闭当前正在编辑的源程序        |
| SerialPortDelegate* serialPort()	                                                                            | 返回当前正在使用的串口对象       |
| codeEditorDelegate* editor()                                                                                  | 返回当前正在使用的代码编辑器对象 |
| void openPlugins(const QString& UUID)	                                                                        | 打开指定的扩展组件              |
| void statusBarMessage(const QString& msg)                                                                     | 在主窗口状态栏显示信息          |
| void stdOutput(OutputType type,const QString& module,const QString& msg )	                                    | 在标准输出窗口显示信息          |
| void systemTrayMessage(const QString& msg,	SystemTrayIcon icon=NoIcon, int millisecondsTimeoutHint =10000) | 显示系统任务栏消息              |
| void viewBoardInformation()	                                                                                | 查看开发板信息                  |
| QStringList fileList(bool fullpath = true)                                                                    | 当前打开的源程序文件清单        |
| QStringList fileHistory()	                                                                                    | 打开的源程序文件历史记录        |
| QStringList projectHistory()	                                                                                | 打开的项目历史记录              |
| void closeAllWindow()                                                                                         | 关闭所有子窗口                  |
| void arrangeTitled()	                                                                                        | 子窗口并排显示                  |
| void arrangeCascade()	                                                                                        | 子窗口层叠显示                  |
| void nextWindow()	                                                                                            | 切换到下一子窗口                |
| void previousWindow()                                                                                         | 切换到上一子窗口                |
| QStringList editorUUIDList()	                                                                                | IDE中注册的编辑器UUID清单       |
| CodeStyle codeEditorStyle()	                                                                                | 代码编辑器样式                  |
| void setCodeEditorStyle(CodeStyle style)                                                                      | 设置代码编辑器样式              |
| QString appName()                                                                                             | 应用程序名称                    |
| QString appVersion()                                                                                          | 应用程序当前版本号              |
| QString appTitle()	                                                                                        | 应用程序标题                    |
| QString appPath()	                                                                                            | 应用程序安装目录                |
| QString dataPath()                                                                                            | 应用程序保存数据文件的目录       |
| void build()                                                                                                  | 构建当前项目（生成）            |
| void clean()                                                                                                  | 清理当前项目                    |
| void cleanAndBuild()                                                                                          | 清理并重新生成                  |

属性读取的用法如：mainwindow.appFont
修改属性的用法如：mainwindow.workspace="d://test_path"

|          属性	           |             说明              |
| -------------------------- | ---------------------------- |
| appFont	                 | 返回应用程序缺省字体           |
| monitorFont                | 返回监视器缺省字体             |
| pythonFont                 | 返回python命令交互窗口缺省字体 |
| stdoutFont                 | 返回控制台字体                |
| codeFont                   | 返回代码编辑器缺省字体         |
| workspace                  | 返回缺省的工作区目录           |
| monitorForegroundColor	 | 返回监视器前景色              |
| monitorBackgroundColor	 | 返回监视器背景色              |

|     枚举类型	     |                          说明                          |
| ---------------- | ------------------------------------------------------ |
| OutputType：     | 调用stdOutput时使用，表示输出的信息在类型                |
|                  | NotDefine = 0,                                         |
|                  | 	Information = 1,                                    |
|                  | 	Warning = 2,                                        |
|                  | 	Critical = 3,                                       |
|                  | 	Question=4                                          |
| SystemTrayIcon : | 调用systemTrayMessage时使用，表示消息汽泡中使用的图标类型 |
|                  | 	NoIcon = 0,                                         |
|                  | 	InformationIcon = 1,                                |
|                  | 	WarningIcon = 2,                                    |
|                  | 	CriticalIcon = 3                                    |
| CodeStyle :      | 编辑器样式                                              |
|                  | 	brightbackground = 0,                               |
|                  | 	darkbackground =1	                                |

### 2、串口

mainwindow.serialPort()返回串口对象，提供以下访问接口：

| 属性	 |            说明            |
| ------- | -------------------------- |
| name	 | 串口的名称，只读，字符串类型 |


|                    方法                    |      	说明      |
| ------------------------------------------ | ----------------- |
| qint32 baudRate()                          | 波特率的设置值     |
| QString stringBaudRate()                   | 波特率的描述文字   |
| QSerialPort::DataBits dataBits()	         | 数据位的设定值     |
| QString stringDataBits()	                 | 数据位的描述文字   |
| QSerialPort::StopBits stopBits()           | 停止位的设定值     |
| QString stringStopBits()	                 | 停止位的描述文字   |
| QSerialPort::FlowControl flowControl()	 | 流控制的设定值     |
| QString stringFlowControl()	             | 流控制的描述文字   |
| QSerialPort::Parity parity()	             | 奇偶校验的设定值   |
| QString stringParity()	                 | 奇偶校验的描述文字 |
| bool isOpen()	                             | 是否已打开         |

### 3、源码编辑器

mainwindow.editor()返回的代码编辑器控件。接口很丰富，并且可以通过连接信号和槽，对代码的一些编辑事件做出响应。具体请参考biForm的相关文档。一些较常能用到的接口如下：

|          调用接口          |           	说明            |
| ------------------------- | --------------------------- |
| setFont(font)             | 	设置缺省字体                |
| setTabWidth(width)	       | 设置tab宽度                  |
| append(text)              | 	在最后追加文本             |
| clear()	               | 清除所有内容                 |
| copy()	                   | 复制所选内容                 |
| cut()	                   | 剪切所选内容                 |
| ensureCursorVisible()	   | 确保当前输入光标可见         |
| ensureLineVisible(line)   | 	确保某行可见              |
| foldAll()	               | 折叠起所有代码块             |
| foldLine(line)	           | 折叠某行                     |
| Indent(line)	           | 指定某行缩进                 |
| Insert(text)	           | 在当前光标处插入文字         |
| insertAt(text,line,index) | 	在指定行、指定位置处插入文字 |
| paste()	               | 从剪切板粘贴                 |
| redo()	                   | 重做上一个操作               |
| removeSelectedText()      | 	删除选中的文本             |
| replaceSelectedText(text) | 	替换选中的文本              |
| selectAll(True)           | 	选择所有                  |
| selectAll(False)          | 	撤消所有选择               |
| setText(text)             | 	设置全部文本                |
| undo()	                   | 撤消上一步操作               |
| unindent(line)	           | 减少指定行的缩进             |
| zoomIn()                  | 	放大                      |
| zoomOut()                 | 	缩小                       |
| length()	               | 总字符数                     |
| lines()	               | 总行数                      |
| text(line)	               | 指定行的文本内容             |
| text(fromline,toline)     | 	指定几行的文本内容          |
| selectedText()            | 	当前选中的文本内容       |
| gotoLine(line)            | 	转到第几行               |
| currentRow()              | 	当前光标所在行             |


可将信号通过connect函数连接到Python函数，比如以下语句：

```Python
mainwindow.editor().connect(‘textChanged’,somePythonFunction)
```

就可以连接文本改变的信号和一个Python函数somePythonFunction，这样，当代码编辑器中的文本发生改变时，调用这个Python函数进行处理。

编辑器控件可用的信号：

|                    信号                    |        说明         |
| ------------------------------------------ | ------------------- |
| cursorPositionChanged(int line, int index) | 光标位置发生改变时   |
| copyAvailable(bool yes)	                 | 可复制文本时         |
| linesChanged()                             | 光标所在行发生改变时 |
| selectionChanged()                         | 选择范围发生改变时   |
| textChanged()                              | 文本内容发生改变时   |

### 4、串口监视器

内置对象 monitor 提供访问串口监视器的接口：

|           调用接口（方法）	            |        说明        |
| -------------------------------------- | ------------------ |
| void setPin(bool checked)	             | 设置是否冻结滚动    |
| void setTimeStamp(bool checked)        | 	设置是否添加时间  |
| void clear()	                         | 清除所有内容        |
| QString text()                         | 	返回所有文本内容 |
| void copy()	                         | 复制所选内容        |
| void zoomIn(int range=1)	             | 放大               |
| void zoomOut(int range=1)	             | 缩小               |
| void connectSerialPort()	             | 连接到串口          |
| void disconnectSerialPort()            | 	断开串口连接      |
| QString textOfLastRow()	             | 最后一行的文本      |
| void sendCommand(const QString& cmd)	 | 发送命令           |
| void setLocalEchoEnabled(bool set)	 | 设置是否本地回显    |


信号的使用方法与mainwindow.editor()类似。比如可以通过

```Python
monitor.connect(‘getData(QByteArray)’,somePythonFunction)
```

在串口接收到数据时，调用somePythonFunction进行处理。

|             信号	             |               说明               |
| ------------------------------- | -------------------------------- |
| getData(const QByteArray& data) | 	串口接收到数据时发出此信号     |
| getLine(const QString &text)	 | 串口接收到一行新的数据时发出此信号 |
| cursorPositionChanged()	     | 光标位置发生改变时                |
| copyAvailable(bool yes)         | 	可复制文本时                  |
| selectionChanged()	             | 选择范围发生改变时                |
| textChanged()	                 | 文本内容发生改变时                |

### 5、项目管理器

“项目管理器”通过 projectManager对象访问，这个对象提供了以下接口：

|                  方法	                  |      说明       |
| ----------------------------------------- | --------------- |
| ProjectDelegate* currentProject()	        | 返回当前项目     |
| void openProject(const QString& filename) | 	打开项目文件 |

|          信号           |       	说明        |
| ----------------------- | ------------------- |
| void projectLoaded()	 | 项目加载后发出此信号 |

### 6、.lsproj开发项目

通过projectManager对象的接口 currentProject() 返回的ProjectDelegate*对象，用于访问这个项目的一些详细信息：

|                      属性                      |                            	说明                            |
| ---------------------------------------------- | ----------------------------------------------------------- |
| empty	                                         | 是否有打开项目，如果没有，这个属性值为True，否则为False，布尔型 |
| name	                                         | 项目名称，字符串类型                                          |
| filename                                       | 	项目文件.lsproj全路径文件名，字符串类型                      |
| workspace                                      | 	项目工作区目录，字符串类型                                |
| includePath	|包含头文件目录清单，字符串列表类型 |                                                             |
| targetFileName|	生成目标文件名，字符串类型    |                                                             |

|            方法             |                               	说明                                |
| --------------------------- | ------------------------------------------------------------------- |
| QVariantList itemList()	 | 项目中包含的所有文件和过滤器清单                                      |
|                             | 返回的值是Qt的QVariantList类型，对应Python中tuple类型，如：           |
|                             | (('项目(sm001)', 1, 'G:/jajabo_dev/bin/workspace/sm001.lsproj', 0), |
|                             | ('sm001.c', 3, 'G:/jajabo_dev/bin/workspace/sm001.c', 1),           |
|                             | ('Makefile', 3, 'G:/jajabo_dev/bin/workspace/Makefile', 1))         |
|                             | 其中每个元素又是一个tuple，分别为“节点标题、类型、对应文件、层级”。     |
|                             | 其中类型为1表示是根节点，为2表示是过滤器，为3表示是文件，为0表示错误。  |

### 7、运行环境

通过envManager对象可以访问IDE运行环境的一些接口：

|            方法	            |           说明           |
| ---------------------------- | ------------------------ |
| QString gccVersion()         | 	检测到的gcc的版本     |
| QString mingw32Version()     | 	检测到的mingw32的版本    |
| bool tftpstarted()	       | 检测tftp是否已启动        |
| QStringList gccIncludePath() | 	返回gcc缺省的包含目录 |
| bool ready()	               | 环境是否已经准备好        |
| void startCheckEnv()	       | 重新检查环境              |

### 8、烧写任务

通过burnManager对象的接口可以添加、删除、执行烧写任务，烧写任务对象类型为BurnTaskDelegate，其提供的接口：

|        属性        |                        	说明                         |
| ------------------ | ------------------------------------------------------ |
| name               | 	任务名称（只读），字符串类型                           |
| filename	         | 待烧写的文件名（可读写），字符串类型                     |
| partition	         | 烧写到分区（可读写），字符串类型                         |
| imageType	         | 镜像文件类型（可读写），返回值是rootfs                   |
| rootFileSystemType | 	根文件系统类型（可读写），返回值是cramfs              |
| args	             | 烧写命令参数（可读写），字符串类型                       |
| commandsAfterBurn	 | 烧写后执行命令，可以设置多条命令（可读写），字符串列表类型 |


|            方法            |             	说明             |
| -------------------------- | ---------------------------- |
| QString toString()	     | 返回可读性更好的任务的描述文字 |
| QStringList commands()	 | 自动生成的命令清单            |
| bool save()	             | 保存对属性的修改              |
| bool remove()	             | 删除这个任务                  |
| bool execute()             | 	执行这个任务               |
| bool isValid()             | 	任务属性设置是否有效       |

### 9、上传模块

通过uploadManager对象访问IDE中的上传模块，其提供的接口：

|                                          方法                                          |                      	说明                       |
| -------------------------------------------------------------------------------------- | ------------------------------------------------ |
| bool uploadFile(const QString& filename, bool exec = false, const QString & args = "", |                                                  |
| 	const QString& toPath = "", bool addExecProperty = false)	                         | 上传文件                                         |
|                                                                                        | 　　　filename 文件名                             |
|                                                                                        | 　　　exec 上传后是否立即执行                      |
|                                                                                        | 　　　args 执行时的参数                           |
|                                                                                        | 　　　toPath 上传到指定目录，为空表示上传到当前目录 |
|                                                                                        | 　　　addExecProperty 是否自动添加可执行属性       |
| QString hostIP()                                                                       | 开发板的IP地址                                   |
| QString tftpServerPath()                                                               | 上位机tftp服务器共享文件目录                      |
| void setLocalhostIP(const QString& ip)                                                 | 设置上位机IP地址                                 |
| void setTftpServerPath(const QString& path)                                            | 设置上位机tftp服务器共享文件目录                  |
| QStringList getLocalHostIPList()	                                                     | 上位机可用的IP地址清单                            |

|                      信号                       |                                	说明                                 |
| ----------------------------------------------- | ---------------------------------------------------------------------- |
| void tftpServerPathChanged(const QString& path) | 	tftp服务器共享文件目录被修改时发出此信号（指通过IDE中的选项设置进行修改） |
| void localHostIPChanged(const QString& ip);	  | 上位机IP地址被修改时发出此信号（指通过IDE中的选项设置进行修改）            |

### 10、烧写模块

通过burnManager对象访问烧写模块，对应的接口： 

|                        方法                        |                              	说明                              |
| -------------------------------------------------- | -------------------------------------------------------------- |
| BurnTaskDelegate* newTask(const QString& name)     | 创建一个新的烧写任务，这个还是个空的任务，还不会被添加到任务列表中 |
| bool removeTask(BurnTaskDelegate* task)	         | 删除掉一个烧写任务                                              |
| BurnTaskDelegate* getTask(const QString& name)	 | 通过名称获取一个烧写任务对象                                     |
| bool executeTask(const QString& name)              | 执行指定名称的烧写任务                                         |
| bool existTask(const QString& name)	             | 通过名称判断是否存在某个烧写任务                                 |

## 在biForm中开发时注意事项

如果需要访问IDE开发的对象，需要先导入IDE模块，使用以下语句：

```Python
from IDE import *
```

但在 biForm 中试运行时，因为试运行环境中并没有IDE这个模块，所以这条语句会报错。

因此可以使用以下方式导入：

```Python
if not this.form.isDebug():
	from IDE import *
```

这样可以在 biForm 中使用试运行功能对其它部分的程序进行调试。但与IDE模块相关的功能，只能在智龙IDE中进行运行调试。可以通过智龙IDE提供的Python命令交互窗口进行调试。

biForm可以选择与智龙IDE不同平台下的版本进行开发，开发后打包生成的PFF本身是可跨平台使用的。

## biForm开发参考

[biForm快速入门](/guides/biform_quickstart)

[biForm开发参考](http://api.bilive.com/#) 

在[下载中心](/download/index)可以下载各个平台的biForm安装程序。

在[参考文档](http://staticpages.bilive.com/#/) 中可以找到更多开发参考文档。

?> 更多文档和信息，请访问公司网站 [bilive.com](https://www.bilive.com)

