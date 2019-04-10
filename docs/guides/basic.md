﻿# 认识 biForm
biForm适合于开发需要图形化用户界面（GUI）的各类桌面应用，以“表单”为所有应用的基本构成单元。开发过程即以创建、调试、打包、发布各种“表单”为基础展开进行。

这种“表单”在biForm中有特定的格式和功能。biForm以XML文件的形式描述了表单上的图形化控件、元数据、及相应的Python脚本等。我们简称为PFF，是便捷式表单文件（Portable Form File）的缩写。

一个PFF文件通常针对一个具体的、相对独立的功能，比如“销售订单.PFF”就是一个用来对销售订单进行增、删、改、查、导出、导入、打印等操作的表单，“销售日报.PFF”就是一个用来对每日销售数据进行汇总统计的表单。一个“表单”最终会以一个PFF文件的形式发布并提供给最终用户。一个或多个“表单”可组合出各种复杂的功能，多个PFF文件也可以打包成为一个完整的应用程序包，以便共同完成更复杂完整的功能。比如“销售订单.PFF”、“销售日报.PFF”、“采购订单.PFF”等多个PFF文件就可以打包成为一个完整的进销存系统。

最终用户通过使用我公司的另一个产品：百利孚软件应用平台V3.1（简称biReader），就能使用这些表单提供的功能。biReader的作用就是为这些表单提供运行时环境，响应用户的操作，与操作系统或数据库管理系统底层进行交互，与其它表单协作和共享数据，组合起来完成各类复杂的功能。

PFF表单本身是与数据库系统、操作系统无关的。一般情况下，同一个PFF表单，在最终用户实际使用时，可以按照自己的需要，选择自己使用的数据库管理系统，表单的功能不会受到影响。但也有些PFF表单是专门针对某类数据库管理系统设计的，那这种表单就只能使用某个特定的数据库管理系统。

目前发布的biForm V3.1和biReaderV3.1支持SQLite、MS SQL Server2000/2005/2008等数据库管理系统。操作系统目前发布的版本仅支持Windows各系列版本。将来会继续发布支持Sybase、Oracle、PostgreSQL，及可在Linux、Mac OS X操作系统上运行的版本。