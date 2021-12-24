# biLive产品安装指南

---

<h2 id=category>目录</h2>

- [Windows版安装](#windows)

- [Linux版安装](#linux)

- [多实例](#instance)

- [工作目录](#workspace)

- [备份和还原](#backup)

---

<h2 id=windows>Windows版安装</h2>

[返回目录](#category)

在Windows系列操作系统上安装，我们一般提供的是MSI文件。

安装过程很简单，只需要按照安装程序的指导逐步操作，就可以完成安装过程了。这里不再详述。	

需要注意的是，Windows有些版本缺省情况下不支持MSI文件，需要先安装Windows Installer。如果是在Windows 7下安装，一般需要先安装vcredist_x86.exe。

### Windows下的安装目录

Windows下安装目录是可以在安装时选择的，但一般建议不要装在C盘。

另外，建议安装目录不要使用中文、空格和一些特殊符号。虽然我们发布的软件都做过测试，正常运行并不受此影响。但因为我们的程序支持使用其它Python第三方库进行扩展，很多第三方Python程序是国外开发的，为了避免新增加的第三方Python库产生不可预知的这类问题，最好使用全英文的安装路径。

---

<h2 id=linux>Linux版安装</h2>

[返回目录](#category)

在Linux操作系统上安装，分几种情况：

### 1、通过操作系统自带的应用商店进行安装

比如deepin/统信UOS，可以直接通过操作系统的“应用商店”一键安装。

### 2、使用DEB 包进行安装

支持DEB包的操作系统，通过命令行安装下载到的.deb文件：

```sudo dpkg -i deb文件名```

### 3、使用RPM包进行安装

支持RPM包的操作系统，通过命令行安装下载到的.rpm文件：

```sudo rpm -ivh rpm文件名 --nodeps --force```

### 4、使用.pkg.tar.zst包进行安装

支持.pkg.tar.zst包的操作系统（如Manjaro），通过命令行安装下载到的文件：

```sudo pacman -U pkg.tar.zst文件名```

### Linux下的安装目录

Linux平台下的安装目录是固定的：

- biForm安装在 /opt/apps/com.bilive.biform

- biForm(龙众创芯专版)安装在 /opt/apps/com.bilive.biformlzcx

- biReader安装在 /opt/apps/com.bilive.bireder

- 智应软件中心安装在 /opt/apps/com.bilive.dziapp

---

<h2 id=instance>多实例</h2>

[返回目录](#category)

智应软件中心不支持多实例，尽管可以在电脑上安装多个副本，但是最终只会有一个运行时实例。

biReader/biForm支持多实例。但Windows安装程序会检查是否已经有安装同一个程序，Linux下则是固定了安装目录，所以通过安装程序是不能得到多个安装副本的。

如果希望安装多个副本，可以将已经安装好的目录复制一份，再适当调整就可以了。

Windows下可以视需要重新添加桌面快捷方式、启动菜单之类的。

Linux下需要修改目录下的“程序名.sh”文件中程序所在路径，并修改“/usr/share/application/包名.desktop”文件中程序所在路径。程序名一般是“biform”、“bireader”这样，包名就是程序名前加上“com.bilive”，如“com.bilive.biform”、“com.bilive.bireader”。

PFF文件与智应软件中心的绑定可以通过操作系统提供的设置文件的“打开方式”来进行设置。

---

<h2 id=instance>.regi 注册文件</h2>

[返回目录](#category)

对于使用.regi文件完成注册激活的软件，.regi文件针对这台机器是长期有效的，所以如果需要重装机器或重新程序，.regi文件需要提前备份出来，安装后再复制回过，通过软件界面重新注册一下。

---

<h2 id=workspace>工作目录</h2>

[返回目录](#category)

工作目录指软件运行时一些临时文件、配置文件、日志文件等存放的位置。

一般用户不需要查看工作目录下的文件。但 biReader 和智应软件中心缺省使用的数据库是在这个目录下的，如果有重要数据需要备份，就可以到这个目录下找。

### 1、Windows下的工作目录

Windows下，如果软件安装在C盘，工作目录是在：“C:\Users\Administrator\”下的一个子目录。不同的软件会有不同的子目录名，比如biForm的社区版用的目录是“.biform_community“，biReader社区版用的目录是“.bireader_web”。

如果软件装在其它盘，工作目录是软件安装所在目录。

### 2、Linux下的工作目录

Linux下使用家目录下的一个子目录做为工作目录，不同软件有不同的子目录名。

比如biForm的社区版用的目录是“~/.biform_community“，biReader社区版用的目录是“~/.bireader_web”。

---

<h2 id=backup>备份和还原</h2>

[返回目录](#category)

如果需要重装系统，通常需要提前备份一些文件，以备重装后还原。

某些情况下，可以直接将软件的安装目录和工作目录全部打包后在新的系统上还原。但象桌面菜单、快捷方式、文件绑定等需要手工处理，或者备份相应的文件后还原。一般建议在新系统上还是使用安装程序后将一些有用的文件复制过去进行还原。

各份和还原也可以在不同的机器上进行操作。比如将智应软件中心的 readerdb.db3 复制到其它电脑上，可以将某一台上的程序和数据完全复制到目标机器上。

提示：如果有重要的数据，比如保险的做法，是建议在Windows下将整个目录和工作目录都备份一份，Linux下将整个工作目录备份一份。如果有使用除缺省数据库之外的其它数据源，也需要进行备份。

### 1、biForm

1）工作目录下的 biForm.Config 文件

如果没有在biForm中设置过项目路径，通常备份这个文件也用处不太大。

2）biform.regi 注册文件

如果是注册过的biForm，这个文件很重要！

### 2、biReader

1）工作目录下的 bireader_history.Config

这个文件中重要的是数据源的设置，如果没有使用过多个数据源，或者想要重新配置，这个文件也可以不用备份。

2）工作目录下的 readerdb.db3

这个是biReader使用的默认数据库。如果没有使用这个数据库，或者没有重要的数据，也不是必须备份的。

3）其它数据库文件

如果使用除默认数据库之外的其它数据源，也需要进行备份。备份操作视使用哪种数据库类型来分别进行处理。

### 3、智应软件中心

1）工作目录下的  readerdb.db3

这个是智应软件中心使用的默认数据库。除非想使用全新的安装，不再想使用以前使用过的程序和数据，否则备份这个文件还是很有必要的。

2）没有了

---

访问[biLive官方网站](https://www.bilive.com)了解更多信息
