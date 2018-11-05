# ���ݿ�ԭ����Ӧ�ý̳���ϰ
>* author:smartZhou
>* speciality:Computer Science and Technology
>* My GitHub:[https://github.com/smartZdw](https://github.com/smartZdw)
>* My blog:[https://www.cnblogs.com/smartZhou](https://www.cnblogs.com/smartZhou)

---

>* ����inventory(�ֿ���)�����ݿ�,���������ݿ�����ļ���Ϊinventory_data,��ʼ��СΪ10MB,��־�ļ���Ϊinventory_log,��ʼ��СΪ2MB.�����ļ�������Ŀ¼C:\DATA\..��,Ҫ��:inventroy_data���Ϊ���޴�,�����ٶ�Ϊ20%,��־�ļ���ʼ��СΪ2MB,���Ϊ5MB, �����ٶ�Ϊ1MB��
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

>* ������
```sql-server 
--����googd��Ʒ��
USE inventory 
GO 
CREATE TABLE goods 
(
	gno char(6)	          NOT NULL PRIMARY KEY,     --��Ʒ���
	gname nvarchar(10)    NOT NULL,					--��Ʒ���� 
	price float			  NOT NULL,					--����
	producer nvarchar(30) NOT NULL					--������
)

--����store�ֿ��
USE inventory 
GO
CREATE TABLE store 
(
	stno char(3)	      NOT NULL PRIMARY KEY,							--�ֿ���	
	address nvarchar(30)  NOT NULL,										--�ֿ��ַ
	telephone varchar(11) CHECK(telephone >= '0' and telephone <= '9'), --�绰
	capacity smallint													--���� 
)

--����invent��������
USE inventory
GO
CREATE	TABLE invent
(
	stno char(3) CONSTRAINT fk_insto FOREIGN KEY REFERENCES store(stno),--�ֿ���	 ���
	gno  char(6) CONSTRAINT fk_ingno FOREIGN KEY REFERENCES goods(gno),	--��Ʒ���   ���
	number int															--������� 
)

--����manager����Ա��
USE inventory 
GO
CREATE TABLE manager
(
	mno char(3) PRIMARY KEY,												--����Ա��� 
	mname nvarchar(10) NOT NULL,											--����Ա���� 
	sex nchar(1) CHECK(sex = '��' or sex = 'Ů'),							--�Ա� 
	birthday date CHECK(birthday >= '1957-1-1' and birthday <= '2010-1-1'),	--��������
	stno char(3) CONSTRAINT fk_mstno FOREIGN KEY REFERENCES store(stno)     --�ֿ���   ���
)

```
>* ��������
```sql-server 
insert into inventory.dbo.goods(gno,gname,price,producer) 
values('bx-179','����',3200,'�ൺ����')
insert into inventory.dbo.goods(gno,gname,price,producer)
values('bx-340','����',2568,'����ѩ��')
insert into inventory.dbo.goods(gno,gname,price,producer)
values('ds-001','����',1580,'�Ĵ�����')
insert into inventory.dbo.goods(gno,gname,price,producer)
values('ds-018','����',2980,'�ൺ����')
insert into inventory.dbo.goods(gno,gname,price,producer)
values('ds-580','����',6899,'�Ͼ���è')
insert into inventory.dbo.goods(gno,gname,price,producer)
values('kt-060','�յ�',3560,'�ൺ����')
insert into inventory.dbo.goods(gno,gname,price,producer)
values('kt-330','�յ�',4820,'�ൺ����')
insert into inventory.dbo.goods(gno,gname,price,producer)
values('xyj-01','ϴ�»�',980,'����С���')
insert into inventory.dbo.goods(gno,gname,price,producer)
values('xyj-30','ϴ�»�',858,'�Ͼ���è')



insert into inventory.dbo.store(stno,address,telephone,capacity)
values('001','1��¥105','89000001',67)
insert into inventory.dbo.store(stno,address,telephone,capacity)
values('002','1��¥106','89000002',78)
insert into inventory.dbo.store(stno,address,telephone,capacity)
values('003','2��¥101','89000003',56)
insert into inventory.dbo.store(stno,address,telephone,capacity)
values('004','2��¥102','89000004',77)
insert into inventory.dbo.store(stno,address,telephone,capacity)
values('005','3��¥104','89000005',80)
insert into inventory.dbo.store(stno,address,telephone,capacity)
values('006','3��¥108','89000006',65)



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
values('101','����','��','1989-2-3 0:00:00','001')
insert into inventory.dbo.manager(mno,mname,sex,birthday,stno)
values('102','����','��','1979-7-23 0:00:00','001')
insert into inventory.dbo.manager(mno,mname,sex,birthday,stno)
values('103','����','��','1978-9-18 0:00:00','002')
insert into inventory.dbo.manager(mno,mname,sex,birthday,stno)
values('104','�ŷ���','Ů','1978-9-12 0:00:00','002')
insert into inventory.dbo.manager(mno,mname,sex,birthday,stno)
values('105','������','��','1990-5-25 0:00:00','003')
insert into inventory.dbo.manager(mno,mname,sex,birthday,stno)
values('106','֣�Ľ�','��','1972-9-6 0:00:00','003')
insert into inventory.dbo.manager(mno,mname,sex,birthday,stno)
values('107','����','��','1989-4-2 0:00:00','004')
insert into inventory.dbo.manager(mno,mname,sex,birthday,stno)
values('108','ղ����','��','1989-7-29 0:00:00','004')
insert into inventory.dbo.manager(mno,mname,sex,birthday,stno)
values('109','��Ʒ��','Ů','1973-9-28 0:00:00','005')
insert into inventory.dbo.manager(mno,mname,sex,birthday,stno)
values('110','������','��','1980-5-3 0:00:00','005')
insert into inventory.dbo.manager(mno,mname,sex,birthday,stno)
values('111','������','��','1980-5-23 0:00:00','006')
insert into inventory.dbo.manager(mno,mname,sex,birthday,stno)
values('101','����','Ů','1978-8-13 0:00:00','006')

```
>* ��ѯ���
```sql-server 
select * from goods
select * from store 
select * from invent 
select * from manager  
```

>* ��ѯЧ��ͼ
![](https://raw.githubusercontent.com/smartZdw/image-/master/img/11.png) ![](https://raw.githubusercontent.com/smartZdw/image-/master/img/12.png)
![invent](https://raw.githubusercontent.com/smartZdw/image-/master/img/13.png)   ![manager](https://raw.githubusercontent.com/smartZdw/image-/master/img/14.png)


