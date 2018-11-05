# 数据库原理与应用教程练习
>* author:smartZhou
>* speciality:Computer Science and Technology
>* My GitHub:[https://github.com/smartZdw](https://github.com/smartZdw)
>* My blog:[https://www.cnblogs.com/smartZhou](https://www.cnblogs.com/smartZhou)

---

>* 创建inventory(仓库库存)的数据库,并设置数据库的主文件名为inventory_data,初始大小为10MB,日志文件中为inventory_log,初始大小为2MB.所有文件都放在目录C:\DATA\..中,要求:inventroy_data最大为无限大,增长速度为20%,日志文件初始大小为2MB,最大为5MB, 增长速度为1MB。
```sql-server
CREATE DATABASE inventory
ON PRIMARY                      --primary data file 
(
	NAME = inventory_data,      --The logical name of the primary file 
	FILENAME = 'C:\Code\DATA\inventory_data.mdf',   --path 
	SIZE = 10 MB,									--initial size
	MAXSIZE = UNLIMITED,							--max size is unlimited 
	FILEGROWTH = 20%								--file growth rate 
)

LOG ON							--log file 
(										
	NAME = 'inventory_log',		--The logical name of the log file 
	FILENAME = 'C:\Code\DATA\inventory_log.ldf',	--path 
	SIZE = 2 MB,									--initial size
	MAXSIZE = 5 MB,									--max size if unlimited 
	FILEGROWTH = 1 MB								--file growth rate 
)

```

>* 创建表
```sql-server 
--创建googd商品表
USE inventory 
GO 
CREATE TABLE goods 
(
	gno char(6)	          NOT NULL PRIMARY KEY,     --商品编号
	gname nvarchar(10)    NOT NULL,					--商品名称 
	price float			  NOT NULL,					--单价
	producer nvarchar(30) NOT NULL					--生产商
)

--创建store仓库表
USE inventory 
GO
CREATE TABLE store 
(
	stno char(3)	      NOT NULL PRIMARY KEY,							--仓库编号	
	address nvarchar(30)  NOT NULL,										--仓库地址
	telephone varchar(11) CHECK(telephone >= '0' and telephone <= '9'), --电话
	capacity smallint													--容量 
)

--创建invent库存情况表
USE inventory
GO
CREATE	TABLE invent
(
	stno char(3) CONSTRAINT fk_insto FOREIGN KEY REFERENCES store(stno),--仓库编号	 外键
	gno  char(6) CONSTRAINT fk_ingno FOREIGN KEY REFERENCES goods(gno),	--商品编号   外键
	number int															--库存数量 
)

--创建manager管理员表
USE inventory 
GO
CREATE TABLE manager
(
	mno char(3) PRIMARY KEY,												--管理员编号 
	mname nvarchar(10) NOT NULL,											--管理员姓名 
	sex nchar(1) CHECK(sex = '男' or sex = '女'),							--性别 
	birthday date CHECK(birthday >= '1957-1-1' and birthday <= '2010-1-1'),	--出生年月
	stno char(3) CONSTRAINT fk_mstno FOREIGN KEY REFERENCES store(stno)     --仓库编号   外键
)

```
>* 插入数据
```sql-server 
insert into inventory.dbo.goods(gno,gname,price,producer) 
values('bx-179','冰箱',3200,'青岛海尔')
insert into inventory.dbo.goods(gno,gname,price,producer)
values('bx-340','冰箱',2568,'北京雪花')
insert into inventory.dbo.goods(gno,gname,price,producer)
values('ds-001','电视',1580,'四川长虹')
insert into inventory.dbo.goods(gno,gname,price,producer)
values('ds-018','电视',2980,'青岛海尔')
insert into inventory.dbo.goods(gno,gname,price,producer)
values('ds-580','电视',6899,'南京熊猫')
insert into inventory.dbo.goods(gno,gname,price,producer)
values('kt-060','空调',3560,'青岛海尔')
insert into inventory.dbo.goods(gno,gname,price,producer)
values('kt-330','空调',4820,'青岛海信')
insert into inventory.dbo.goods(gno,gname,price,producer)
values('xyj-01','洗衣机',980,'无锡小天鹅')
insert into inventory.dbo.goods(gno,gname,price,producer)
values('xyj-30','洗衣机',858,'南京熊猫')



insert into inventory.dbo.store(stno,address,telephone,capacity)
values('001','1号楼105','89000001',67)
insert into inventory.dbo.store(stno,address,telephone,capacity)
values('002','1号楼106','89000002',78)
insert into inventory.dbo.store(stno,address,telephone,capacity)
values('003','2号楼101','89000003',56)
insert into inventory.dbo.store(stno,address,telephone,capacity)
values('004','2号楼102','89000004',77)
insert into inventory.dbo.store(stno,address,telephone,capacity)
values('005','3号楼104','89000005',80)
insert into inventory.dbo.store(stno,address,telephone,capacity)
values('006','3号楼108','89000006',65)



insert into inventory.dbo.invent(stno,gno,number)
values('004','bx-179',5)
insert into inventory.dbo.invent(stno,gno,number)
values('002','bx-179',12)
insert into inventory.dbo.invent(stno,gno,number)
values('003','bx-340',10)
insert into inventory.dbo.invent(stno,gno,number)
values('001','ds-001',20)
insert into inventory.dbo.invent(stno,gno,number)
values('003','ds-018',8)
insert into inventory.dbo.invent(stno,gno,number)
values('001','ds-018',16)
insert into inventory.dbo.invent(stno,gno,number)
values('002','ds-580',15)
insert into inventory.dbo.invent(stno,gno,number)
values('004','kt-060',9)
insert into inventory.dbo.invent(stno,gno,number)
values('001','kt-060',13)
insert into inventory.dbo.invent(stno,gno,number)
values('004','xyj-01',10)
insert into inventory.dbo.invent(stno,gno,number)
values('003','xyj-30',21)


insert into inventory.dbo.manager(mno,mname,sex,birthday,stno)
values('101','张力','男','1989-2-3 0:00:00','001')
insert into inventory.dbo.manager(mno,mname,sex,birthday,stno)
values('102','李明','男','1979-7-23 0:00:00','001')
insert into inventory.dbo.manager(mno,mname,sex,birthday,stno)
values('103','王辉','男','1978-9-18 0:00:00','002')
insert into inventory.dbo.manager(mno,mname,sex,birthday,stno)
values('104','张凤玉','女','1978-9-12 0:00:00','002')
insert into inventory.dbo.manager(mno,mname,sex,birthday,stno)
values('105','刘晓宏','男','1990-5-25 0:00:00','003')
insert into inventory.dbo.manager(mno,mname,sex,birthday,stno)
values('106','郑文杰','男','1972-9-6 0:00:00','003')
insert into inventory.dbo.manager(mno,mname,sex,birthday,stno)
values('107','明宇','男','1989-4-2 0:00:00','004')
insert into inventory.dbo.manager(mno,mname,sex,birthday,stno)
values('108','詹虎新','男','1989-7-29 0:00:00','004')
insert into inventory.dbo.manager(mno,mname,sex,birthday,stno)
values('109','李品慧','女','1973-9-28 0:00:00','005')
insert into inventory.dbo.manager(mno,mname,sex,birthday,stno)
values('110','刘利华','男','1980-5-3 0:00:00','005')
insert into inventory.dbo.manager(mno,mname,sex,birthday,stno)
values('111','王文宇','男','1980-5-23 0:00:00','006')
insert into inventory.dbo.manager(mno,mname,sex,birthday,stno)
values('101','王玮','女','1978-8-13 0:00:00','006')

```
>* 查询结果
```sql-server 
select * from goods
select * from store 
select * from invent 
select * from manager  
```

>* 查询效果图
-----------
![](https://raw.githubusercontent.com/smartZdw/image-/master/img/11.png) 
![](https://raw.githubusercontent.com/smartZdw/image-/master/img/12.png)
-----------
![invent](https://raw.githubusercontent.com/smartZdw/image-/master/img/13.png) 
![manager](https://raw.githubusercontent.com/smartZdw/image-/master/img/14.png)


