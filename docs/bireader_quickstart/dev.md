# biReader快速入门 - 二次开发和扩展

[返回目录](/bireader_quickstart)

所有 biReader 中运行的PFF程序都是使用本公司的开发工具 biForm 开发的。用户如果有编程基础或有自己的开发团队，完全可以自己进行二次开发。

biReader 中所有PFF程序都是“即插即用”的，由多个PFF程序组装组合而成复杂的系统。所以每个用户都可以有自己的组合方式，能充分满足用户的个性化需求。开发者不必等到所有用户进行统一升级的时候再发布新版本，也不必每次发布都发布完整的一个系统，能很容易地针对某个用户的特殊需求进行局部升级，并且不会影响到其他用户。用户的个性化需求能得到更加及时响应的同时，也不会给开发者带来额外的负担。

## 扩展和升级

### 1）修改或升级现有PFF程序

指需要修改原有PFF程序的某些功能。需要有源码才能进行这样的修改和发布。

biReader 中如果打开新的PFF程序文件，如果它在原来的系统中已经存在，则会判断其版本号是否大于现有的版本号，如果是，则对旧版本进行升级。系统管理员 admin 有权限制普通用户进行升级。

### 2）增加新PFF程序

指在原有的系统中添加新的PFF程序实现对旧系统的扩展。

biReader 中如果打开新的PFF程序文件，如果它在原来的系统中不存在，就能实现这种扩展。系统管理员 admin 有权控制普通用户进行这种扩展。

### 3）废止掉某些PFF程序

有些情况下，希望停用某些已经在 biReader 中注册的PFF程序。

可以通过在程序中控制用户权限，禁用使用某些PFF程序来实现。也可以直接在数据库中删除掉对应的PFF程序信息。

### 4）扩展Python第三方库

我们发布的安装程序中已经集成了很多 Python 第三方库，用户也可以使用自己的库或其它第三方库进行扩展。只需要将相关的文件复制到 sys.path 对应的目录下就可以在程序中导入这些扩展库。

## 与其它PFF程序进行数据集成

如果需要与其它PFF程序进行数据集成，只需要向开发团队获取相关的数据表结构文件就可以了。在 biForm 中导入需要使用的数据结构信息，以此为基础发布的PFF程序，能自然地与原系统进行数据集成。

## 与其它系统进行集成

biReader 用于 C/S 架构的应用开发时，经常会需要与其它系统进行集成，实际上 biReader 也很适合这类应用的开发。

如何集成需要按照原有系统使用的数据库类型及集成需求等进行综合考虑，以下是两种常用的集成方式：

### 方式一：与其它系统使用同一个MSSQL服务器

在原系统的同一个 MSSQL 服务器上新建一个数据库，biReader连接这个新数据库，在程序中访问 ```this.form.database()``` 对象，通过它提供的接口，执行SQL语句访问其它数据库。比如：

``` Python
db=this.form.database()
#查询数据
re=db.execute("select top 10 * from otherdb.dbo.tablename")
log.debug(re)
#跨库查询
re=db.execute("select t0.ID as ID,t0.fname as fname,sum(t0.fcount) as fcount1,sum(t1.fcount) as fcount2 "+\
    " from otherdb.dbo.tablename t0 join "+db.getRealTableName('t_person')+ " t1 on t0.ID=t1.ID "+\
    " group by t0.ID,t0.fname")
#修改数据
db.execute("update otherdb.dbo.tablename set field1='1' where field1='0'")
```

这种方式读写其它数据库、跨库查询都很方便。

### 方式二：使用 DatabaseConnection 类创建数据源

如果不能在原系统所在服务器上新建数据库，可以在程序中使用 DatabaseConnection 类连接其数据库来实现访问。比如：

``` Python
#创建连接对象
db=this.form.addDBConnection('connection_erp')
#连接 postgresql 数据源
connectOK=db.connectPostgreSQL('192.168.1.8','erp','postgres','password',5432)
if connectOK:#连接成功
    #查询数据
	result=db.execute("select fname,fcount from tablename where field1=0")
	log.debug(result)
	db.execute("update tablename set field1='1' where field1='0'")
```

使用这种方式，对外接库查询、修改数据都方便，但是与 biReader 自身使用的数据库或其它数据源进行跨库查询就不太方便。但biReader后台可以使用多种数据库类型，可以使用本地的SQLite数据库，也可以在局域网中与其它用户共享一个 MSSQL 或 PostgreSQL 数据库，与原系统使用的数据库类型就没关系了，只取决于 biReader 本身运行产生的数据是否需要与其它人共享。

[返回目录](/bireader_quickstart)

