# biLive运行时引擎

**biLive跨平台应用框架**（以下简称**biLive框架**）是武汉百利孚信息科技有限公司推出的一个桌面程序应用框架，用于开发跨平台桌面应用程序。

biLive框架发布的程序是我们自己设计的一种文件格式，并不是通常所见的可执行程序（EXE文件）。所以，在目标机器上需要由**biLive运行时引擎**提供支撑才能运行。

biLive运行时引擎是以DLL和SO文件形式发布的，通常是随支持biLive应用框架的应用程序一起安装到目标机器。
-
目前已经使用biLive运行时引擎的应用程序包括：

## [biForm](http://api.bilive.com/#/)

为开发PFF程序提供的集成式开发环境，也使用了biLive运行时引擎为调试程序提供环境。

## [biReader通用运行时环境](/bireader/bireader)

biReader是一个允许以多实例方式运行的通用的运行时环境。每个实例为使用这一实例的所有PFF提供一个统一的多文档主窗口环境。不同实例之间的PFF的运行是互相隔离的，也即同一实例中的PFF直接通过PFF运行时引擎进行集成和协同，也可以通过biLive框架中的分布式引擎与其它用户处的其它实例进行集成和协同。

PFF在biReader中是即插即用的，用户随时可以对某个PFF进行升级，或添加新的PFF对原有功能进行扩展。

biReader可使用多种ＤＢＭＳ做为后台数据库，多个终端用户可以通过使用同一个数据源共享数据、同步升级、权限管理等。所以它适合为ＥＲＰ／ＣＲＭ／ＨＲ等管理信息系统提供独立的运行环境。

一个biReader实例可使用多个数据源并在它们之间进行切换，适合用户需要并行使用多个系统的场合。

因为它支持多实例，所以也适合用于为测试和调试PFF程序提供独立的互相隔离的运行环境。

## [智应软件中心](/dziapp/dziapp)

智应软件中心是一个单实例的通用的运行时环境。与biReader最大的不同是，每个PFF以独立的窗口运行。并且智应后台目前只能使用SQLite数据库，暂时不能使用其它数据库。

在ＰＣ端只需要安装智应软件中心，即可双击启动运行PFF／PFP文件。智应软件中心为这些PFF提供统一的运行时环境和管理工具。

## 智龙IDE

为龙芯嵌入式开发提供集成式开发环境，使用了biLive框架做为插件框架，PFF程序做为插件为它提供扩展功能。

## 管理信息系统

biLive框架从2007开始研发，从2010年开始在多个项目中投入了使用。以下是一些典型的应用系统：

- 店铺销售模拟系统
- 进销存模拟系统
- 仓库库位管理系统
- 财务总账查询分析系统
- 备查财务管理系统
- 政府采购协议供货采购系统
- 社会治理人员信息平台
- 医疗补助报销管理系统
- 固定资产卡片管理系统
- 公费医疗管理系统
- 征收项目管理系统
- 工程项目及资金管理办公平台
- 出纳管理系统
- 工资核算管理系统

## 下载和安装

[下载中心](/download/index)
