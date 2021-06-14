# 总结一下

第一个基于 biForm 的表单 **我的书籍** 就完成了。

我们为了达到针对“我的书籍”增删改查等所有功能，其实一共只写了 **23** 行Python代码。

在 biForm 中，可以查看这个表单的完整脚本，一共230行。其中包括 biForm 自动生成的脚本，以及我们自己写的脚本。

我们可以看到用 biForm 开发这类表单，效率是非常高的。

一般我们是不需要通过查看完整的脚本来调试PFF表单的。但是有时如果脚本运行出错，可以通过 Python 的错误提示中的行号，在“完整脚本”中定位到出错的位置。

查看一下整个表单完整的脚本，可以帮助我们了解 biForm 中各个函数之间是怎么样的关系，以便有时需要使用公共变量和公用函数时，知道如何能使它正常运作。

完整的脚本如下：

``` python 
# -*- coding: cp936 -*-
import logging
import base64
import codecs, sys
pub = sys.modules.get('__main__').pub
log = sys.modules.get('__main__').log
# ----------------------------------- -----------------------------------
class formclass_myBook(object):
	'''表单类：myBook 版本：9'''
	record=None
	def __init__(self,formobj):
		self.form = formobj
		self.label1 = formobj.object('label1')
		self.leAuthor = formobj.object('leAuthor')
		self.cbClass = formobj.object('cbClass')
		self.leISBN = formobj.object('leISBN')
		self.spYear = formobj.object('spYear')
		self.leName = formobj.object('leName')
		self.label = formobj.object('label')
	def vscrollto(self,x):
		return self.form.vscrollto(x)
	def hscrollto(self,x):
		return self.form.hscrollto(x)

# ----------------------------------- -----------------------------------
this = formclass_myBook(myBook)

class objMaintable:
	'''表单：t_book 的主表的记录对象'''
	db = this.form.database()
	tableName = this.form.mainTable
	pkName=''
	isEmpty = True
	def reset(self) :
		self.ID = 0
		self.fname = ''
		self.fauthor = ''
		self.fclass = ''
		self.fyear = 0
		self.fISBN = ''
		self.UUID = ''
		self.lastUpdated = ''
		self.isEmpty = True
		self.data = ()

	def refresh(self,UUID='') :
		self.reset()
		if self.pkValue is None and len(UUID)==0:
			return
		self.fieldList = self.db.getFieldList(self.tableName)
		if len(UUID)!=0 :
			re = self.db.data(self.tableName, self.fieldList, "UUID='%s'" % (UUID))
		elif self.db.fieldIsTextType(self.tableName, self.pkName) :
			re = self.db.data(self.tableName, self.fieldList, "%s='%s'" % (self.pkName, self.pkValue))
		else :
			re = self.db.data(self.tableName, self.fieldList, '%s=%s' % (self.pkName, self.pkValue))
		if len(re) != 0 :
			self.data = re[0]
			self.isEmpty = False
			self.ID = self.data[0]
			self.fname = self.data[1]
			self.fauthor = self.data[2]
			self.fclass = self.data[3]
			self.fyear = self.data[4]
			self.fISBN = self.data[5]
			self.UUID = self.data[6]
			self.lastUpdated = self.data[7]

	def __init__(self,pkValue=None,UUID=''):
		self.pkValue = pkValue
		if self.tableName is not None:
			pk = self.db.getPrimaryKey(self.tableName)
			if len(pk)>0:
				self.pkName = pk[0]
		if pkValue is None and len(UUID)>0:
			self.refresh(UUID=UUID)
		else:
			self.refresh()

	def subRecord(self,subTableName,fieldList=()) :
		if self.isEmpty or not subTableName in self.db.tablesName():
			return []
		if len(fieldList)==0:
			fieldList= self.db.getFieldList(subTableName)
		fields=','.join(fieldList)
		return self.db.execute(f"select {fields} from "+self.db.getRealTableName(subTableName)+ " where UUID='"+self.UUID+"'")

	def ID_updated(self):
		if self.pkName=='ID':
			return (self.ID==0 or self.ID=='')
		try:
			return not DB_t_book_ID_bindvalue()==self.ID
		except:
			return False
	def fname_updated(self):
		if self.pkName=='fname':
			return (self.fname==0 or self.fname=='')
		try:
			return not DB_t_book_fname_bindvalue()==self.fname
		except:
			return False
	def fauthor_updated(self):
		if self.pkName=='fauthor':
			return (self.fauthor==0 or self.fauthor=='')
		try:
			return not DB_t_book_fauthor_bindvalue()==self.fauthor
		except:
			return False
	def fclass_updated(self):
		if self.pkName=='fclass':
			return (self.fclass==0 or self.fclass=='')
		try:
			return not DB_t_book_fclass_bindvalue()==self.fclass
		except:
			return False
	def fyear_updated(self):
		if self.pkName=='fyear':
			return (self.fyear==0 or self.fyear=='')
		try:
			return not DB_t_book_fyear_bindvalue()==self.fyear
		except:
			return False
	def fISBN_updated(self):
		if self.pkName=='fISBN':
			return (self.fISBN==0 or self.fISBN=='')
		try:
			return not DB_t_book_fISBN_bindvalue()==self.fISBN
		except:
			return False

this.record = objMaintable(None)
# ----------------------------------- -----------------------------------
def form_loadrecord(record_uuid):
	'''这段脚本会在获得一条主表的记录，需要在表单上显示时调用。
	参数 record_uuid 是这条记录的UUID。您可以通过 this.record 引用这条记录对象。'''
	this.record=objMaintable(UUID=record_uuid)
	if this.record.isEmpty:
		return
	record=this.record.data
	log.info('#form:Load record')
	#--*<debugtag>*-- 0;form;Load record
	#以下写入你自己的脚本:
	this.leName.text = this.record.fname
	this.leAuthor.text = this.record.fauthor
	this.cbClass.setCurrentText(this.record.fclass)
	this.leISBN.text = this.record.fISBN
	this.spYear.value = this.record.fyear
	
	return None

# ----------------------------------- -----------------------------------
def form_beforesave():
	'''这段脚本将在保存表单数据前被调用。如果允许保存，则返回 True，否则返回 False。'''
	result = True
	log.info('#form:Before save')
	#--*<debugtag>*-- 0;form;Before save
	#以下写入你自己的脚本:
	if len(this.leName.text)==0:
		pub.infoMsgBox(this.form.caption,'书名不得为空！')
		return False
	if len(this.leAuthor.text)==0:
		pub.infoMsgBox(this.form.caption,'作者不得为空！')
		return False
	if len(this.leISBN.text)==0:
		pub.infoMsgBox(this.form.caption,'ISBN不得为空！')
		return False

	return result

# ----------------------------------- -----------------------------------
#myBook 的公共模块
#用这段脚本定义公用的变量和函数.
log.info('* 开始加载表单：myBook')
log.info('myBook::Public module')
#--*<debugtag>*-- 0;form;:Public module
#以下写入你自己的脚本:


#-----------------------------------
def DB_t_book_fyear_bindvalue(index=1):
	'''这段脚本会在需要向表的字段写入值时调用.
	参数 'index' 是记录的序号,由 1 开始记数.返回待写入的值.'''
	result = ''
	log.info('#t_book_fyear:Bind value')
	#--*<debugtag>*-- 5;t_book_fyear;Bind value
	#以下写入你自己的脚本:
	result = this.spYear.value
	return result
#-----------------------------------
def DB_t_book_fISBN_bindvalue(index=1):
	'''这段脚本会在需要向表的字段写入值时调用.
	参数 'index' 是记录的序号,由 1 开始记数.返回待写入的值.'''
	result = ''
	log.info('#t_book_fISBN:Bind value')
	#--*<debugtag>*-- 5;t_book_fISBN;Bind value
	#以下写入你自己的脚本:
	result = this.leISBN.text
	return result
#-----------------------------------
def DB_t_book_fauthor_bindvalue(index=1):
	'''这段脚本会在需要向表的字段写入值时调用.
	参数 'index' 是记录的序号,由 1 开始记数.返回待写入的值.'''
	result = ''
	log.info('#t_book_fauthor:Bind value')
	#--*<debugtag>*-- 5;t_book_fauthor;Bind value
	#以下写入你自己的脚本:
	result = this.leAuthor.text
	return result
#-----------------------------------
def DB_t_book_fclass_bindvalue(index=1):
	'''这段脚本会在需要向表的字段写入值时调用.
	参数 'index' 是记录的序号,由 1 开始记数.返回待写入的值.'''
	result = ''
	log.info('#t_book_fclass:Bind value')
	#--*<debugtag>*-- 5;t_book_fclass;Bind value
	#以下写入你自己的脚本:
	result = this.cbClass.currentText
	return result
#-----------------------------------
def DB_t_book_fname_bindvalue(index=1):
	'''这段脚本会在需要向表的字段写入值时调用.
	参数 'index' 是记录的序号,由 1 开始记数.返回待写入的值.'''
	result = ''
	log.info('#t_book_fname:Bind value')
	#--*<debugtag>*-- 5;t_book_fname;Bind value
	#以下写入你自己的脚本:
	result = this.leName.text
	return result
```
