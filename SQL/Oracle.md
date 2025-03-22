## 一、Mac 安装 Oracle 数据库
Oracle 没有提供 Mac 系统安装程序, 需要使用 docker 容器安装 Oracle
[MacOS 系统如何安装 Oracle11g 企业级数据库](https://blog.csdn.net/bai_mi_student/article/details/118409004)
## 二、Oracle 服务器的结构
- Oracle 数据库：数据的集合（永久的）
	- 物理结构：数据文件
	   - 数据文件 `.dbf`  存储数据（表、索引） 
	   - 日志文件 `.log`  记录对数据库的操作
	   - 控制文件 `.ctl`  记录数据库的结构
	- 逻辑结构：逻辑概念
		- 数据库 --> 表空间 --> 区 --> 段 --> 块 --> 数据
	- 理解：
		1. 物理地址：长沙市开福区蔡锷北路司马里 38 号
		2. 逻辑概念：学校 --> 年级 --> 班级 --> 小组 -->个人

## 三、用户管理

1. sys 超级管理员，用于管理和维护整个数据库，必须以 sysdba 登录
2. system 管理员，用于管理和维护用户及权限
3. 自定义用户，<font color="#f79646">必须授权后才能使用</font>

- 常用命令：
	1. 显示当前用户 show user;
	2. 切换用户 conn 用户名/密码@网络服务名 [as sysdba];
	3. 断开连接 disc;
	4. 清除屏幕 clear scr;
	5. 锁定用户 alter user 用户名 account lock;
	6. 解锁用户 alter user 用户名 account unlock;
	7. 修改密码 alter user 用户名 identified by 密码;
	8. 创建用户 create user 用户名 identified by 密码;
	9. 删除用户 drop user 用户名 [cascade];

## 四、权限管理
1. 系统权限，针对数据库的操作（create, drop, alter）
2. 对象权限，针对数据库对象的操作（表的增删改查, 过程的调用）
3. 角色，一组权限的捆绑，用于简化权限管理
	- connect 连接的角色
	- resource 使用资源的角色
	- dba 管理员的角色


- 常用命令：
	1. 授予权限
		- `grant 系统权限 to 用户名;`
		- `grant 对象权限 on 用户名.对象名 to 用户名;`
	2. 撤销权限
		- `revoke 系统权限 from 用户名;`
		- `revoke 对象权限 on 用户名. 对象名 from 用户名;`
	3. 表空间授权
	   -  `ALTER USER "用户名" QUOTA UNLIMITED ON "表空间";`
	4. 授权查询其他用户
		- `grant select on scott.dept to accp;`

>[!example]- 案例: 创建 scott 用户并且将 `scott.dmp` 数据导入该用户下
> - 创建 `scott` 用户, 并授权
> 	```sql
> 	create user scott identified by admin; # 创建用户
> 	grant all privileges to SCOTT; # 授权
> 	```
> - <font color="#ff0000">tip: 在 docker 容器上使用的 Oracle, 宿主环境和容器不互通</font>
> 	1. 先将 `scott.dmp` 拷贝到容器环境
> 		```shell
> 		docker cp /Users/fishx/Downloads/2/scott.dmp  oracle11g:/home
> 		```
> 	2. 再进入 docker 容器, 进入目标目录, 执行 Oracle 导入语句 
> 		```sql
> 		imp scott/admin file=scott.dmp ignore=y full=y
> 		```

[docker oracle 创建空间表，创建数据库](https://www.cnblogs.com/achengmu/p/13043685.html)

>[!example]- 案例: 导出 `stuDBA` 用户下的表为 `school.dmp`
> 1. 先为该用户赋予相关权限
> 	```shell
> 	sqlplus /nolog 
> 	connect /as sysdba 
> 	## 授权用户 stuDBA，拥有连接、管理员、导入、导出权限，并可以传递权限
> 	grant connect,dba,exp_full_database,imp_full_database to stuDBA with admin option;  
> 	```
> 2. 创建表空间
> 	```sql
> 	-- 查询临时表空间的路径
> 	select name from v$tempfile; 
> 	
> 	-- 创建表空间
> 	create tablespace lygj
> 	datafile '/home/oracle/app/oracle/oradata/helowin/temp02.dbf'
> 	size 100m
> 	autoextend on
> 	next 10m;
> 	
> 	-- 创建用户，并选择表空间
> 	create user lygj
> 	identified by 123456
> 	default tablespace lygj;
> 	 
> 	-- 授权登录
> 	grant dba to lygj;
> 	```
> 3. 进入容器, 数据导出
> 	```shell
> 	docker exec -it 9461643d6383 /bin/bash
> 	
> 	expdp stuDBA/stu dumpfile=studba.dmp directory=DATA_PUMP_DIR full=y
> 	```
> 4. 拷贝到系统路径下
> 	```shell
> 	docker cp 9461643d6383:/u01/app/oracle/admin/XE/dpdump/school.dmp /Users/fishx/
> 	```



在 Oracle 中进行分页查询，可以使用 ROWNUM 伪列和子查询的方式来实现。具体步骤如下：

1. 使用 SELECT 语句查询需要进行分页的数据，并使用 ORDER BY 语句对查询结果进行排序，例如：

   
```sql
SELECT * FROM table_name ORDER BY column_name;
```


2. 使用子查询的方式，将原始查询结果作为子查询，并使用 ROWNUM 伪列来对查询结果进行分页，例如：

   
```sql
SELECT * FROM (SELECT * FROM table_name ORDER BY column_name) WHERE ROWNUM >= start_row AND ROWNUM <= end_row;
```


其中，start_row 和 end_row 分别表示需要查询的起始行和结束行。例如，如果需要查询第 1 行到第 10 行的数据，可以设置 start_row 为 1，end_row 为10。

需要注意的是，Oracle 的 ROWNUM 是在返回结果集时动态计算的，因此在子查询中使用 ROWNUM 时，需要先进行排序，否则返回的结果可能不是按照要求的顺序排序的。

另外，如果查询结果超过了 Oracle 默认设置的最大行数（1000 行），需要使用 ROWNUM 设置查询结果的最大行数，例如：

   
```sql
SELECT * FROM (SELECT * FROM table_name WHERE ROWNUM <= 1000 ORDER BY column_name) WHERE ROWNUM >= start_row AND ROWNUM <= end_row;
```


这样可以保证查询结果不会超过最大行数，并且能够对查询结果进行分页。