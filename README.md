# mysql
mysql for 5.7
创建用户：  create user 'springdev'@'%' identified by 'springdev_mysql';
创建数据库：create database springdev default charset 'utf8mb4' default collate utf8mb4_general_ci;
授权用户：  grant all privileges on springdev.* to 'springdev'@'%' identified by 'springdev_mysql';
刷新：      flush privileges;


用户
查看所有用户: select host,user from mysql.user;
创建用户：create user 'springdev'@'%' identified by 'springdev_mysql';
删除用户：drop user 'springdev'@'%';  delete from user where user='springdev' and Host='%';
修改用户：rename user springdev to newuser；

权限
查看用户权限：show grants for 'springdev'@'%';
赋予用户权限：grant all privileges on springdev.* to 'springdev'@'%' identified by 'springdev_mysql';
回收用户权限：revoke all privileges on springdev.* from 'springdev'@'%';

密码
修改用户密码：update mysql.user set authentication_string=PASSWORD('Root_123456*0987') where User='root';
              alter user 'root'@'%' identified by 'Root_123456*0987';
              set password for 'springdev'@'%' = PASSWORD('springdev_123');

数据库
查看所有数据库:show databases;
创建数据库：create database springdev default charset 'utf8mb4' default collate utf8mb4_general_ci;
删除数据库：drop database springdev;
修改数据库：alter database springdev default character set 'utf8mb4' default collate utf8mb4_general_ci;

数据表
查看所有数据表: show tables;
显示数据表结构: describe tablename;  desc tablename;　show columns from tablename;
创建数据表：create table tablename(id int not null auto_increment,name char(20) not null,address char(50) null,city char(50) null,age int not null,love char(50) not null default 'No habbit',primary key(id))engine=InnoDB default charset 'utf8mb4' default collate utf8mb4_general_ci;
删除数据表：drop table tablename;
修改数据表： 
             更改表名: rename table 原表名 to 新表名;
             修改字段: update 表名 set 字段名 = replace(字段名, '旧内容', '新内容');
             增加字段: alter table 表名 add 字段 类型 其他;
                       alter table tablename add passtest int(4) default '0'
             查询数据: select * from tablename;                         
             插入数据: insert into <表名> [(<字段名 1>[,..<字段名 n>])] values (值 1)[,(值 n)]
                       insert into tablename values(1,'Tom',96.45),(2,'Joan',82.99), (2,'Wang',96.59);
             修改数据: update 表名 set 字段=新值,... where 条件
                       update tablename set name='Mary' where id=1;  
             删除数据：delete from 表名 where 表达式
                       delete from tablename where id=1;                             

查看正在连接的线程: show full processlist;
查看当前打开的表：show open tables;
查看服务器状态: show status like '%lock%';
查看innodb引擎的运行时信息: show engine innodb status\G;
查看服务器配置参数: show variables like '%timeout%';


#查询数据库连接
show full  processlist;
show status like '%Max_used_connections%';
show status like '%Threads_connected%';    #当前连接数
show status like '%table_lock%';           #表锁定
show status like 'innodb_row_lock%';       #行锁定
show status like '%qcache%';               #查询缓存情况
show variables like "%query_cache%";
SHOW STATUS LIKE 'Qcache%';
show variables like "%binlog%";
show status like 'Aborted_clients';        #由于客户没有正确关闭连接已经死掉，已经放弃的连接数量
show variables like '%max_connections%';   #查看最大连接数量
show variables like '%timeout%';           #查看超时时间
show variables like 'log_%';               #查看日志是否启动
 

一、导出数据库用mysqldump命令（注意mysql的安装路径，即此命令的路径）：
1、导出整个数据库
mysqldump -u用户名 -p密码 数据库名 > 数据库名.sql
mysqldump -uroot -p123 dbname > dbname.sql

2.导出一个表
mysqldump -u用户名 -p密码 数据库名 表名 > 数据库名_表名.sql
mysqldump -uroot -p dbname users > dbname_users.sql

3、只导出表结构
mysqldump -u用户名 -p密码 -d 数据库名 > 数据库名.sql
mysqldump -uroot -p123 -d dbname > dbname.sql

4.导出一个数据库结构
mysqldump -uroot -p123 -d --add-drop-table dbname > dbname_db.sql
-d 没有数据 --add-drop-table 在每个create语句之前增加一个drop table

二、导入数据库
1、首先建空数据库
mysql>create database dbname default charset 'utf8mb4' default collate utf8mb4_general_ci;

2、导入数据库
方法一：
mysql>use dbname;
mysql>source /home/abc/dbname.sql;
方法二：
mysql -u用户名 -p密码 数据库名 (表名) < 数据库名.sql
mysql -uroot -p123 dbname (users) < dbname.sql



SQL: Structure Query Language

	DDL: CREATE, DROP, ALTER
	DML: SELECT, INSERT, UPDATE, DELETE
	DCL: GRANT, REVOKE

	事务：
		ACID
			A: 原子性
			C: 一致性
			I：隔离性
			D：持久性

		提交：持久
		未提交：提交，回滚


	隔离：隔离级别
		读未提交：read uncommitted
		读提交：  read committed
		可重读：  repeatable read
		串行化：  serializable

	MySQL: 存储引擎
		MyISAM: 无事务
			非聚集
		InnoDB: 事务型
			聚集索引

	机械式硬盘：
		随机读写
		顺序读写

	关系型数据库设计前三范式：
		字段的原子性
		主键
		非主属性不允许重复

	SQL: 规范，ANSI
		SQL-86， SQL-89， SQL-92， SQL-99， SQL-03

	关系数据库的约束：主键，外键，惟一键，条件约束，非空约束

	MySQL的安装方式：
		源码编译
		rpm包：
			OS Vendor
			MySQL
		通用二进制格式

初始化：提供配置文件
	
	配置文件.cnf
		集中式的配置:多个应用程序共用的配置文件
			[mysqld]
			[mysqld_safe]
			[client]

		# /usr/local/mysql/bin/mysqld --help --verbose | head -20

			Default options are read from the following files in the given order:
				/etc/mysql/my.cnf  /etc/my.cnf  ~/.my.cnf 

			使用配置文件的方式
				1、它依次查找每个需要查找的文件，结果是所有文件并集；
				2、如果某参数在多个文件中出现多次，后读取的最终生效；

		# /usr/local/mysql/bin/mysqld --help --verbose
			1、显示mysqld程序启动时可用的选项，通常都是长选项
			2、显示mysqld的配置文件中可用的服务变量
				mysql> SHOW GLOBAL VARIABLES
				mysql> SHOW SESSION VARIABLES

初始化：第二个操作
	1、删除所有匿名用户
		mysql> DROP USER ''@'localhost';
		mysql> DROP USER ''@'www.magedu.com';

		用户帐号由两部分组成：username@host
			host还可以使用通配符：
				%: 任意长度的任意字符
				_: 匹配任意单个字符

	2、给所有的root用户设定密码:
		第一种方式：
			mysql> SET PASSWORD FOR username@host = PASSWORD('your_passwrod');

		第二种方式：
			mysql> UPDATE user SET password = PASSWORD('your_password') WHERE user = 'root';
			mysql> FLUSH PRIVILEGES;

		第三种方式：
			# mysqladmin -uUserName -hHost password 'new_password' -p
			# mysqladmin -uUserName -hHost -p flush-privileges

连入MySQL服务器
	mysql client <--mysql protocol--> mysqld

		mysqld接收连接请求：
			本地通信：客户端与服务器端位于同一主机，而且还要基于127.0.0.1(localhost)地址或lo接口进行通信；
				Linux OR Unix: Unix Sock, /tmp/mysql.sock, /var/lib/mysql/mysql.sock
				Windows: memory, pipe
			远程通信：客户端与服务器位于不同的主机，或在同一主机便使用非回环地址通信
				TCP socket

		客户端工具：mysql, mysqladmin, mysqldump, mysqlcheck
			[client]

			通行的选项：
				-u, --user=
				-h, --host=
				-p, --password=
				--protocol={tcp|socket|memory|pipe}
				--port=
				--socket=    例如：/tmp/mysql.sock


			mysql监听的端口： 3306/tcp

		非客户端类的管理工具：myisamchk, myisampack

	mysql工作模式: 
		交互式模式
			mysql> 
		脚本模式
			mysql < /path/to/mysql_script.sql

	mysql交互式模式：
		客户端命令
			mysql> help
			mysql> \?
				\c
				\g
				\G
				\q
				\!
				\s
				\. /path/to/mysql_script.sql
		服务器端命令：需要命令结束符，默认为分号(;)
			mysql> help contents

			mysql> help Keryword

	mysql命令行选项：
		--compress
		--database=, -D 
		-H, --html：输出结果为html格式的文档
		-X, --xml: 输出格式为xml
		--sate-updates: 拒绝使用无where子句的update或delete命令；

	mysql命令提示符：
		mysql> 等待输入命令
		->
		'>
		">
		`>
		/*> 

	mysql的快捷键：
		Ctrl + w: 删除光标之前的单词
		Ctrl + u: 删除光标之前至命令行首的所有内容
		Ctrl + y: 粘贴使用Ctrl+w或Ctrl+u删除的内容
		Ctrl + a: 移动光标至行首
		Ctrl + e: 移动光标至行尾


	mysqldmin工具：
		mysqladmin [options] command [arg] [command [arg]] ...

			command: 
				create DB_NAME
				drop DB_NAME
				debug: 打开调试日志并记录于error log中；

				status：显示简要状态信息
					--sleep #: 间隔时长
					--count #: 显示的批次

				extended-status: 输出mysqld的各状态变量及其值，相当于执行“mysql> SHOW GLOBAL STATUS”
				variables: 输出mysqld的各服务器变量

				flush-hosts: 清空主机相关的缓存：DNS解析缓存，此前因为连接错误次数过多而被拒绝访问mysqld的主机列表
				flush-logs: 日志滚动，二进制日志和中继日志
				refresh: 相当于同时使用flush-logs和flush-hosts

				flush-privileges: 
				reload: 功能同flush-privileges

				flush-status: 重置状态变量的值

				flush-tables: 关闭当前打开的表文件句柄

				flush-threads：清空线程缓存

				kill:　杀死指定的线程，可以一次杀死多个线程，以逗号分隔，但不能有多余空格

				password: 修改当前用户的密码；

				ping: 

				processlist：显示mysql线程列表

				shutdown: 关闭mysqld进程；

				start-slave
				stop-slave: 启动/关闭从服务器线程

	GUI客户端工具：
		Navicat for mysql
		Toad for mysql
		mysql front
		sqlyog
		phpMyAdmin

	总结：mysql初始化，mysql配置文件读取次序，mysql初始用户，mysql客户端命令，mysqldmin，GUI


开发DBA：数据库设计(E-R关系图)、SQL开发、内置函数、存储例程(存储过程和存储函数)、触发器、事件调度器(event scheduler)
管理DBA：安装、升级，备份、恢复，用户管理、权限管理，监控、分析、基准测试，语句优化(SQL语句)，数据字典，按需要配置服务器（服务器变量：MyISAM, InnoDB, 缓存, 日志）

SQL语言组成部分：
	DDL: 
	DML: 
	完整性定义语言：DDL的一部分功能	
		主键、外键、惟一键、条件、非空、事务
	视图定义：虚表，存储下来的SELECT语句
	事务控制：
	嵌入式SQL和动态SQL：
	DCL：授权

数据类型的功用：
	1、存储的值类型；
	2、占据的礁存储空间；
	3、定长，变长；
	4、如何被索引及排序；
	5、是否能够被索引；

数据字典：系统编目(system catalog)
	保存数据库服务器上的元数据

	元数据：
		关系的名字
		每个关系的各字段的名字
		各字段的数据类型和长度
		约束
		每个关系上的视图的名字及视图的定义

		授权用户的名字
		用户的授权和帐户信息

		统计类的数据：
			每个关系字段的个数；
			每个关系中行数；
			每个关系的存储方法；

		保存元数据的数据库：
			information_schema
			mysql
			performance_shcema

数据类型：
	字符型
		char
		varchar
		binary
		varbinary
		text
		blob
	数值型
		精确数值型
			整型
			十进制数据：decimal
		近似数值型
			单精度浮点型
			双精度浮点型
	日期时间型
		日期型
		时间型
		日期时间型
		时间戳
	布尔型

	内建类型
		ENUM, SET

	数值型：
		TINYINT
		SMALLINT
		MEDIUMINT
		INT
		BIGINT
		DECIMAL
		FLOAT
		DOUBAL

		BIT

	字符型：
		CHAR
		VARCHAR
		TINYTEXT
		TEXT
		MEDIUMTEXT
		LONGTEXT

		BINARY
		VARBINARY
		TINYBLOB
		BLOB
		MEDIUMBLOB
		LONGBLOB

		ENUM
		SET

	日期时间型：
		DATE
		TIME
		DATETIME
		TIMESTAMP
		YEAR

	CHAR、VARCHAR和TEXT几种字符型常用的属性修饰符：
		NOT NULL：非空约束
		NULL：允许为空
		DEFAULT 'string'：默认值，不适用于TEXT类型
		CHARACTER SET '字符集'
			mysql> SHOW VARIABLES LIKE '%char%';
			mysql> SHOW CHARACTER SET
		COLLATION '规则': 排序规则
			mysql> SHOW COLLATION;

	BINARY、VARBINARY和BLOB几种字符型常用的属性修饰符：
		NOT NULL
		NULL
		DEFAULT: 不适用于BLOB

	整型的常用属性修饰符：
		AUTO_INCREMENT：自动增长
			前提：非空，且惟一；支持索引，非负值；
		UNSIGNED：无符号
		NULL
		NOT NULL
		DEFAULT 

	浮点型常用修饰符：
		NOT NULL
		NULL
		DEFAULT
		UNSIGNED

	日期时间型的修饰符：
		NOT NULL
		NULL
		DEFAULT

	ENUM和SET的修饰符：
		NOT NULL
		NULL
		DEFAULT ''

MySQL SQL_MODE: SQL模式
	TRADITIONAL, STRICT_TRANS_TABLES, or STRICT_ALL_TABLES


	设定服务器变量的值：（仅用于支持动态的变量）
		支持修改的服务器变量：
			动态变量：可以MySQL运行时修改
			静态变量：于配置文件中修改其值，并重启后方能生效；

		服务器变量从其生效范围来讲，有两类：
			全局变量：服务器级别，修改之后仅对新建立的会话有效；
			会话变量：会话级别，仅对当前会话有效；
				会话建立时，从全局继承各变量；

		查看服务器变量：
			mysql> SHOW [{GLOBAL|SESSION}] VARIABLES [LIKE ''];
			mysql> SELECT @@{GLOBAL|SESSION}.VARILABLE_NAME;
			mysql> SELECT * FROM INFORMATION_SCHEMA.GLOBAL_VARIABLES WHERE VARIABLE_NAME='SOME_VARIABLE_NAME';
			mysql> SELECT * FROM INFORMATION_SCHEMA.SESSION_VARIABLES WHERE VARIABLE_NAME='SOME_VARIABLE_NAME';

		修改变量
			前提：默认仅管理员有权限修改全局变量

			mysql> SET {GLOBAL|SESSION} VARIABLE_NAME='VALUE'; 

		注意：无论是全局还是会话级别的动态变量修改，在重启mysqld后都会失效；想永久有效，可定义在配置文件中的相应段中[mysqld]；


	MySQL中字符大小写：
		1、SQL关键字及函数名不区分字符大小写；
		2、数据库、表及视图名称的大小区分与否取决于低层OS及FS
		3、存储过程、存储函数及事件调度器的名字不区分大小写，但触发器区分；
		4、表别名区分大不写；
		5、对字段中的数据，如果字段类型为Binary类型，则区分大小写；非Binary不区分大小写；

	数据库：
		CREATE {DATABASE|SCHEMA} [IF NOT EXISTS] db_name [DEFAULT] [CHARACTER SET=''] [DEFAULT] [COLLATE='']

		DROP {DATABASE | SCHEMA} [IF EXISTS] db_name

		ALTER {DATABASE|SCHEMA} db_name [DEFAULT] [CHARACTER SET=''] [DEFAULT] [COLLATE='']

	表：
		表创建：第一种方式
			CREATE [TEMPORARY] TABLE [IF NOT EXISTS] tbl_name
		    (create_definition,...)
		    [table_options]

		    (create_definition,...)：
		    	字段的定义：字段名、类型和类型修饰符
		    	键、约束或索引：
		    		PRIMARY KEY, UNIQUE KEY, FOREIGN KEY, CHECK
		    		{INDEX|KEY} 

		    [table_options]
		    	ENGINE [=] engine_name
		    		mysql> SHOW ENGINES;
		    	AUTO_INCREMENT [=] value
		    	[DEFAULT] CHARACTER SET [=] charset_name
		    	[DEFAULT] COLLATE [=] collation_name
		    	COMMENT [=] 'string'
		    	DELAY_KEY_WRITE [=] {0 | 1}
		    	ROW_FORMAT [=] {DEFAULT|DYNAMIC|FIXED|COMPRESSED|REDUNDANT|COMPACT}
		    	TABLESPACE tablespace_name [STORAGE {DISK|MEMORY|DEFAULT}]



		    	MyISAM表，每表有三个文件，都位于数据库目录中：
		    		tb_name.frm: 表结构定义
		    		tb_name.MYD: 数据文件
		    		tb_name.MYI: 索引文件

		    	InnoDB表，有两种存储方式
		    		1、默认：每表有一个独立文件和一个多表共享的文件
		    			tb_name.frm: 表结构的定义，位于数据库目录中；
		    			ibdata#: 共享的表空间文件，默认位于数据目录(datadir指向的目录)中；

		    		2、独立的表空间：
		    			tb_name.frm: 每表有一个表结构文件
		    			tb_name.ibd: 一个独有的表空间文件

		表创建：第二种方式（复制表数据）
			CREATE [TEMPORARY] TABLE [IF NOT EXISTS] tbl_name
			    [(create_definition,...)]
			    [table_options]
			    select_statement

		表创建：第三种方式（复制表结构）
			CREATE [TEMPORARY] TABLE [IF NOT EXISTS] tbl_name
    		{ LIKE old_tbl_name | (LIKE old_tbl_name) }

    表删除：
    	DROP [TEMPORARY] TABLE [IF EXISTS]
	    tbl_name [, tbl_name] ...
	    [RESTRICT | CASCADE]

	表修改：
		ALTER TABLE tbl_name
   			 [alter_specification [, alter_specification] ...]

   		修改字段定义：
   			插入新字段
   			删除字段
   				DROP [COLUMN] col_name
   			修改字段
   				修改字段名称
   					CHANGE [COLUMN] old_col_name new_col_name column_definition
        			[FIRST|AFTER col_name]
   				修改字段类型及属性等
   					 MODIFY [COLUMN] col_name column_definition
        			[FIRST | AFTER col_name]
   		修改约束、键或索引：

   		表改名：
   			 RENAME [TO|AS] new_tbl_name

   			 mysql> RENAME TABLE old_name TO new_name; 

   		指定排序标准的字段：
   			 ORDER BY col_name [, col_name] ...

   		转换字符集及排序规则：
   			CONVERT TO CHARACTER SET charset_name [COLLATE collation_name]

   		表选项修改：
   			[table_options]
		    	ENGINE [=] engine_name
		    		mysql> SHOW ENGINES;
		    	AUTO_INCREMENT [=] value
		    	[DEFAULT] CHARACTER SET [=] charset_name
		    	[DEFAULT] COLLATE [=] collation_name
		    	COMMENT [=] 'string'
		    	DELAY_KEY_WRITE [=] {0 | 1}
		    	ROW_FORMAT [=] {DEFAULT|DYNAMIC|FIXED|COMPRESSED|REDUNDANT|COMPACT}
		    	TABLESPACE tablespace_name [STORAGE {DISK|MEMORY|DEFAULT}]



练习题：
新建如下表（包括结构和内容）：
ID    Name          Age     Gender     Course
1     Ling Huchong   24      Male       Hamogong
2     Huang Rong    19      Female     Chilian Shenzhang
3     Lu Wushaung   18      Female     Jiuyang Shenggong
4     Zhu Ziliu     52      Male       Pixie Jianfa
5     Chen Jialuo   22      Male       Xianglong Shiba Zhang
6	  Ou Yangfeng   70      Male       Shenxiang Bannuo Gong

1、新增字段：
	Class 字段定义自行选择；放置于Name字段后；

2、将ID字段名称修改为TID;

3、将Age字段放置最后；


MySQL的查询操作：
	单表查询：简单查询
	多表查询：连续查询
	联合查询：

	选择和投影：
		投影：挑选要显示的字段
		选择：挑选符合条件的行

			投影：SELECT 字段1, 字段2, ... FROM tb_name;
				  SELECT * FROM tb_name;

			选择：SELECT 字段1, ... FROM tb_name WHERE 子句;
					布尔条件表达式

		布尔条件表达式操作符：
			=
			<=>
			<>
			<
			<=
			>
			>=

			IS NULL
			IS NOT NULL

			LIKE: 支持的通配符: %(任意长度的任意字符)， _（任意单个字符）

			RLIKE, REGEXP: 支持使用正则表达式

			IN: 判断指定字段的值是否在给定在列表中；

			BETWEEN ... AND ...: 位于指定的范围之间

		组合条件测试：
			NOT, !
			AND, &&
			OR, ||

		聚合函数：
			SUM(), AVG(), MAX(), MIN(), COUNT()



	练习：导入hellodb.sql，以下操作在students表上执行
		1、以ClassID分组，显示每班的同学的人数；
		2、以Gender分组，显示其年龄之和；
		3、以ClassID分组，显示其平均年龄大于25的班级；
		4、以Gender分组，显示各组中年龄大于25的学员的年龄之和；


	SELECT语句的执行流程：
		FROM clause --> WHERE clause --> GROUP BY --> HAVING clause --> ORDER BY ... --> SELECT --> LIMIT

	SELECT语句:
		DISTINCT：指定的结果相同的只显示一次；
		SQL_CACHE：缓存于查询缓存中；
		SQL_NO_CACHE：不缓存查询结果；


MySQL多表查询和子查询：
	
	联结查询：事先将两张或多张表join，根据join的结果进行查询；

		cross join: 交叉联结
			(a+b)(c+d+e)=

		自然联结：
			等值联结

		外联结：
			左外联结：只保留出现在左外连接运算之前（左边）的关系中的元组；
				left_tb LEFT JOIN right_tb ON 连接条件
			右外联结：只保留出现在右外连接运算之后（右边）的关系中的元组；
				left_tb RIGHT JOIN right_tb ON 连接条件
			全外联结

		自联结：

	别名：
		表别名
		字段别名


		练习：导入hellodb.sql，完成以下题目：
		1、显示前5位同学的姓名、课程及成绩；
		2、显示其成绩高于80的同学的名称及课程；
		3、求前8位同学每位同学自己两门课的平均成绩，并按降序排列；
		4、显示每门课程课程名称及学习了这门课的同学的个数；


		思考：
		1、如何显示其年龄大于平均年龄的同学的名字？
		2、如何显示其学习的课程为第1、2，4或第7门课的同学的名字？
		3、如何显示其成员数最少为3个的班级的同学中年龄大于同班同学平均年龄的同学？

		4、统计各班级中年龄大于全校同学平均年龄的同学。

	子查询：在查询中嵌套的查询
		用于WHERE中的子查询
			1、用于比较表达式中的子查询
				子查询的返回值只能有一个；
			2、用于EXISTS中的子查询
				判断存在与否
			3、用于IN中的子查询；
				判断存在于指定列表中
		用于FROM中子查询：
			SELECT alias.col,... FROM (SELECT clause) AS alias WHERE condition

		MySQL不擅长于子查询：应该避免使用子查询；

	总结：MySQL的联结查询及子查询
		联结：
			交叉联结
			内联结
			外联结
				左外
				右外
			自联结
		子查询：
			用于WHERE中的子查询
				用于条件比较：子查询只能一个值
				用于IN：子查询可以返回多个值
				EXISTS：子查询可以返回多个值
			用于FROM子句的子查询

MySQL的联合查询：SELECT clauase UNION SELECT clause UNION ...
	把两个或多个查询语句的结果合并成一个结果进行输出；

MySQL视图:
	存储下来的SELECT语句；


	DDL:
		DATABASE
		TABLE
		VIEW

	DML:
		SELECT
		INSERT/REPLACE
		UPDATE
		DELETE

	INSERT INTO:
		第一种：
			INSERT INTO tb_name [(col1, col2,...)] {VALUES|VALUE} (val1, val2,...)[,(val21,val22,...),...]

		第二种：
			INSERT INTO tb_name SET col1=val1, col2=val2, ...

		第三种：
			INSERT INTO tb_name SELECT clause

			REPLACE的工作机制：与INSERT相同，除了在新插入的数据与表中的主键或惟一索引定义的数据相同会替换老的行；


	UPDATE：
		UPDATE [LOW_PRIORITY] [IGNORE] table_reference
		    SET col_name1=val1 [, col_name2=val2] ...
		    [WHERE where_condition]
		    [ORDER BY ...]
		    [LIMIT row_count]

		UPDATE通常情况下，必须要使用WHERE子句，或者使用LIMIT限制要修改的行数；

		--safe-updates

	DELETE:
		DELETE [LOW_PRIORITY] [QUICK] [IGNORE] FROM tbl_name
		    [WHERE where_condition]
		    [ORDER BY ...]
		    [LIMIT row_count]

		TRUNCATE tb_name

	explain

MySQL锁：
	
	执行操作时施加的锁模式
		读锁：共享锁
		写锁：独占锁，排它锁

	锁粒度：
		表锁：table lock
			锁定了整张表
		行锁：row lock
			锁定了需要的行

			粒度越小，开销越大，但并发性越好；
			粒度越大，开销越小，但并发性越差；

	锁的实现位置：
		MySQL锁：可以使用显式锁
		存储引擎锁：自动进行的（隐式锁）；

		显式锁(表级锁)：
			LOCK TABLES
			UNLOCK TABLES

			LOCK TABLES
			    tbl_name lock_type
			    [, tbl_name lock_type] ...

			    锁类型：READ|WRITE

			InnoDB存储引擎也支持另外一种显式锁(锁定挑选出的部分行，行级锁 )：
				SELECT ... LOCK IN SHARE MODE;
				SELECT ... FOR UPDATE;

事务：Transaction
	事务就是一组原子性的查询语句，也即将多个查询当作一个独立的工作单元。

	ACID测试：能满足ACID测试就表示其支持事务，或兼容事务。
		A：Atomicity，原子性
		C：Consistency, 一致性
		I: Isolation, 隔离性, 一个事务的所有修改操作在提交之前对其它事务是不可见的
			Tom: 7000-3000
			Jerry: 5000+3000
		D：Durability, 持久性, 一旦事务得到提交，其所做的修改会永久有效

	隔离级别：
		READ UNCOMMITTED (读未提交)
			脏读，不可重读，幻读
		READ COMMITTED (读提交)
			不可重读，幻读
		REPEATABLE READ (可重读)
			幻读
		SERIALIZABLE (可串行化)
			强制事务的串行执行避免了幻读；

	跟事务相关的常用命令

		mysql> START TRANSACTION
		mysql> COMMIT

		mysql> ROLLBACK

		mysql> SAVEPOINT identifier
		mysql> ROLLBACK [WORK] TO [SAVEPOINT] identifier

	如果没有显式启动事务，每个语句都会当作一个独立的事务，其执行完成后会被自动提交；
		mysql> SELECT @@global.autocommit;
		mysql> SET GLOBAL autocommit = 0;

		关闭自动提交，请记得手动启动事务，手动进行提交；

	查看MySQL的事务隔离级别
		mysql> SHOW GLOBAL VARIABLES LIKE 'tx_isolation';
		mysql> SELECT @@global.tx_isolation;

	建议：对事务要求不特别严格的场景下，可以使用读提交；

	MVCC：多版本并发控制
		每个事务启动时，InnoDB为会每个启动的事务提供一个当下时刻的快照；
			为了实现此功能，InnoDB会为每个表提供两隐藏的字段，一个用于保存行的创建时间，一个用于保存行的失效时间；
				里面存储的是系统版本号；（system version number）

		只在两个隔离级别下有效：READ COMMITTED和REPEATABLE READ


MySQL存储引擎：	存储引擎也通常称作“表类型”
		
	mysql> SHOW ENGINES;
	mysql> SHOW TABLES STATUS [LIKE clause] [WHERE clause]

	SHOW TABLE STATUS [{FROM | IN} db_name]
    [LIKE 'pattern' | WHERE expr]

		 mysql> SHOW TABLE STATUS IN hellodb WHERE Name='classes'\G
		*************************** 1. row ***************************
		           Name: classes
		         Engine: InnoDB
		        Version: 10
		     Row_format: Compact
		           Rows: 8
		 Avg_row_length: 2048
		    Data_length: 16384
		Max_data_length: 0
		   Index_length: 0
		      Data_free: 9437184
		 Auto_increment: 9
		    Create_time: 2014-04-08 11:14:52
		    Update_time: NULL
		     Check_time: NULL
		      Collation: utf8_general_ci
		       Checksum: NULL
		 Create_options: 
		        Comment: 
		1 row in set (0.01 sec)   

    	Name: 表名
    	Engine: 存储引擎
    	Version: 版本
    	Row_format: 行格式
    		{DEFAULT|DYNAMIC|FIXED|COMPRESSED|REDUNDANT|COMPACT}
    	Rows: 表中的行数
    	Avg_row_length: 平均每行所包含的字节数；
    	Data_length: 表中数据总体大小，单位是字节
    	Max_data_length: 表能够占用的最大空间，单位为字节
    	Index_length: 索引的大小，单位为字节
    	Data_free: 对于MyISAM表，表示已经分配但尚未使用的空间，其中包含此前删除行之后腾出来的空间
    	Auto_increment: 下一个AUTO_INCREMENT的值；
    	Create_time: 表的创建时间；
    	Update_time：表数据的最近一次的修改时间；
    	Check_time：使用CHECK TABLE或myisamchk最近一次检测表的时间；
    	Collation: 排序规则
    	Checksum: 如果启用，则为表的checksum；
    	Create_options: 创建表时指定使用的其它选项；
    	Comment: 表的注释信息

    InnoDB:
    	两种格式
    		1、innodb_file_per_table=OFF，即使用共享表空间
    			每张表一个独有的格式定义文件: tb_name.frm
    			还一个默认位于数据目录下共享的表空间文件：ibdata#

    		2、innodb_file_per_table=ON，即使用独立表空间
    			每个表在数据库目录下存储两个文件：
    				tb_name.frm
    				tb_name.ibd
    MyISAM:
    	每个表都在数据库目录下存储三个文件：
    		tb_name.frm
    		tb_name.MYD
    		tb_name.MYI

    表空间：table space，由InnoDB管理的特有格式数据文件，内部可同时存储数据和索引

    如何修改默认存储引擎：通过default_storage_engine服务变量实现

    各存储引擎的特性：
   	 	InnoDB：
   	 		事务：事务日志
   	 		外键：
   	 		MVCC：
   	 		聚簇索引：
   	 			聚簇索引之外的其它索引，通常称为辅助索引
   	 		行级锁：间隙锁
   	 		支持辅助索引
   	 		支持自适应hash索引
   	 		支持热备份

   	 	MyISAM:
   	 		全文索引
   	 		压缩：用于实现数据仓库，能节约存储空间并提升性能
   	 		空间索引
   	 		表级锁
   	 		延迟更新索引

   	 		不支持事务、外键和行级锁
   	 		崩溃后无法安全恢复数据

   	 		适用场景：只读数据、较小的表、能够容忍崩溃后的修改操作和数据丢失

   	 	ARCHIVE：
   	 		仅支持INSERT和SELECT，支持很好压缩功能；
   	 		适用于存储日志信息，或其它按时间序列实现的数据采集类的应用；

   	 		不支持事务，不能很好的支持索引；

   	 	CSV：
   	 		将数据存储为CSV格式；不支持索引；仅适用于数据交换场景；

   	 	BLACKHOLE：
   	 		没有存储机制，任何发往此引擎的数据都会丢弃；其会记录二进制日志，因此，常用于多级复制架构中作中转服务器；

   	 	MEMORY：
   	 		保存数据在内存中，内存表；常用于保存中间数据，如周期性的聚合数据等；也用于实现临时表
   	 		支持hash索引，使用表级锁，不支持BLOB和TEXT数据类型

   	 	MRG_MYISAM：是MYISAM的一个变种，能够将多个MyISAM表合并成一个虚表；

   	 	NDB：是MySQL CLUSTER中专用的存储引擎

   	第三方的存储引擎：

   		OLTP类：
	   		XtraDB: 增强的InnoDB，由Percona提供；
	   			编译安装时，下载XtraDB的源码替换MySQL存储引擎中的InnoDB的源码

	   		PBXT: MariaDB自带此存储引擎
	   			支持引擎级别的复制、外键约束，对SSD磁盘提供适当支持；
	   			支持事务、MVCC

	   		TokuDB: 使用Fractal Trees索引，适用存储大数据，拥有很压缩比；已经被引入MariaDB；

   		列式存储引擎：
   			Infobright: 目前较有名的列式引擎，适用于海量数据存储场景，如PB级别，专为数据分析和数据仓库设计；
   			InfiniDB
   			MonetDB
   			LucidDB

   		开源社区存储引擎：
   			Aria：前身为Maria，可理解为增强版的MyISAM(支持崩溃后安全恢复，支持数据缓存)
   			Groona：全文索引引擎，Mroonga是基于Groona的二次开发版
   			OQGraph: 由Open Query研发，支持图结构的存储引擎
   			SphinxSE: 为Sphinx全文搜索服务器提供了SQL接口
   			Spider: 能数据切分成不同分片，比较高效透明地实现了分片(shared)，并支持在分片上支持并行查询；

   	索引类型：

   	 	聚簇索引
   	 	辅助索引

   	 	B树索引
   	 	R树索引
   	 	hash索引
   	 	全文索引



   	如何选择？
   		是否需要事务
   		备份的类型的支持
   		崩溃后的恢复
   		特有的特性

MySQL用户管理：
	
	用户帐号：username@hostname, password

		用户帐号管理：
			CREATE USER
			DROP UESER
			RENAME USER
			SET PASSWORD

		权限管理：
			GRANT 
			REVOKE

	CREATE USER

		CREATE USER username@hostname
		    [
		        IDENTIFIED BY [PASSWORD] 'password'
		    ]

		主机也可使用通配符：
			%：
			_:

		testuser@'172.16.100.1__'
		   172.16.100.100-172.16.100.199

		查看用户能够使用的权限：SHOW GRANTS FOR username@'hostname'

	MySQL的权限类别：
		库级别
		表级别
		字段级别
		管理类
		程序类

		管理类权限：
			CREATE TEMPORARY TABLES
			CREATE USER
			FILE
			SUPER
			SHOW DATABASES
			RELOAD
			SHUTDOWN
			REPLICATION SLAVE
			REPLICATION CLIENT
			LOCK TABLES
			PROCESS

		库级别和表级别：
			ALTER
			ALTER ROUTINE
			CREATE 
			CREATE ROUTINE
			CREATE VIEW
			DROP
			EXECUTE
			GRNAT OPTION
			INDEX
			SHOW VIEW

		数据操作（表级别）：
			SELECT
			INSERT
			UPDATE
			DELETE

		字段级别：
			SELECT(col1,...)
			UPDATE(col1,...)
			INSERT(col1,...)

		所有权限：
			ALL [PRIVILEGES]


	GRANT ALL ON [FUNCTION] *.* 

		GRANT priv_type [(column_list)]
		      [, priv_type [(column_list)]] ...
		    ON [TABLE|FUNCTION|PROCEDURE] priv_level
		    TO username@hostname [IDENTIFIED BY 'password'], [username@hostname [],...]
		    [REQUIRE SSL]
		    [WITH with_option ...]

		priv_level:
		    *
		  | *.*
		  | db_name.*
		  | db_name.tbl_name
		  | tbl_name
		  | db_name.routine_name

		with_option:
		    GRANT OPTION
		  | MAX_QUERIES_PER_HOUR count
		  | MAX_UPDATES_PER_HOUR count
		  | MAX_CONNECTIONS_PER_HOUR count
		  | MAX_USER_CONNECTIONS count

	收回授权：
		REVOKE
		    priv_type [(column_list)]
		      [, priv_type [(column_list)]] ...
		    ON [object_type] priv_level
		    FROM user [, user] ...

		REVOKE ALL PRIVILEGES, GRANT OPTION
		    FROM user [, user] ...

	几个跟用户授权相关的表：
		db: 库级别权限；
		host: 主机级别权限，已废弃
		tables_priv: 表级别权限
		colomns_priv：列级别的权限
		procs_priv：存储过程和存储函数相关的权限
		proxies_priv：代理用户权限

	练习：
		1、授予testuser能够通过172.16.0.0/16网络内的任意主机访问当前mysql服务器的权限；
		2、让此用户能够创建及删除testdb数据库，及库中的表；
		3、让此用户能够在testdb库中的t1表中执行查询、删除、更新和插入操作；
		4、让此用户能够在testdb库上执行创建和删除索引；
		5、让此用户能够在testdb.t2表上查询id和name字段，并允许其将此权限转授予其他用户；


MySQL查询缓存

	用于保存MySQL查询语句返回的完整结果。被命中时，MySQL会立即返回结果，省去解析、优化和执行等阶段。

	如何检查缓存？
		MySQL保存结果于缓存中：
			把SELECT语句本身做hash计算，计算的结果作为key，查询结果作为value

	什么样的语句不会被缓存？
		查询语句中有一些不确定数据时，不会缓存：例如NOW(), CURRENT_TIME()；一般来说，如果查询中包含用户自定义函数、存储函数、用户变量、临时表、mysql库中系统表、或者任何包含权限的表，一般都不会缓存；

	缓存会带来额外开销：
		1、每个查询都得先检查是否命中；
		2、查询结果要先缓存；

		MariaDB [(none)]> SHOW GLOBAL VARIABLES LIKE 'query_cache%';
		+------------------------------+----------+
		| Variable_name                | Value    |
		+------------------------------+----------+
		| query_cache_limit            | 1048576  |
		| query_cache_min_res_unit     | 4096     |
		| query_cache_size             | 16777216 |
		| query_cache_strip_comments   | OFF      |
		| query_cache_type             | ON       |
		| query_cache_wlock_invalidate | OFF      |
		+------------------------------+----------+

		query_cache_type: 查询缓存类型；是否开启缓存功能，开启方式有三种{ON|OFF|DEMAND}；
			DEMAND：意味着SELECT语句明确使用 SQL_CACHE 选项时才会缓存；

		query_cache_size: 总空间，单位为字节，大小必须是1024的整数倍。MySQL启动时，会一次分配并立即初始化这里指定大小的内存空间；这意味着，如果修改此大小，会清空缓存并重新初始化的。

		query_cache_min_res_unit: 存储缓存的最小内存块；(query_cache_size-Qcache_free_memory)/Qcache_queries_in_cache能够获得一个理想的值。

		query_cache_limit: 单个缓存对象的最大值，超出时则不预缓存；手动使用SQL_NO_CACHE可以人为地避免尝试缓存返回结果超出此参数限定值的语句。

		query_cache_wlock_invalidate: 如果某个表被其它用户连接锁住了，是否仍然从缓存中返回结果。OFF表示返回。


	如何判断命令率：
		MariaDB [hellodb]> SHOW GLOBAL STATUS LIKE 'Qcache%';
		+-------------------------+----------+
		| Variable_name           | Value    |
		+-------------------------+----------+
		| Qcache_free_blocks      | 1        |
		| Qcache_free_memory      | 16757008 |
		| Qcache_hits             | 4        |
		| Qcache_inserts          | 2        |
		| Qcache_lowmem_prunes    | 0        |
		| Qcache_not_cached       | 18       |
		| Qcache_queries_in_cache | 2        |
		| Qcache_total_blocks     | 6        |
		+-------------------------+----------+	

		碎片整理：FLUSH QUERY_CACHE
		清空缓存：RESET QUERY_CACHE

		计算命中率：
			MariaDB [hellodb]> SHOW GLOBAL STATUS WHERE Variable_name='Qcache_hits' OR Variable_name='Com_select';
			+---------------+-------+
			| Variable_name | Value |
			+---------------+-------+
			| Com_select    | 24    |
			| Qcache_hits   | 4     |
			+---------------+-------+

			Qcache_hits/(Com_select+Qcache_hits)

			也应该参考另外一个指标：命中和写入的比率，即Qcache_hits/Qcache_inserts的值，此比值如果能大于3:1，则表明缓存也是有效的。能达到10:1，为比较理想的情况。

	缓存优化使用思路：
		1、批量写入而非多次单个写入；
		2、缓存空间不宜过大，因为大量缓存同时失效时会导致服务器假死；
		3、必要时，使用SQL_CACHE和SQL_N0_CACHE手动控制缓存；
		4、对写密集型的应用场景来说，禁用缓存反而能提高性能；


MySQL日志：
	查询日志
	慢查询日志：查询执行时长超过指定时长的查询，即为慢查询
	错误日志
	二进制日志：复制功能依赖于此日志
	中继日志：
	事务日志：
		随机I/O转换为顺序I/O
		ACID：持久性

		日志文件组：至少应该有两个日志文件；
		注意：尽可能使用小事务以提升事务引擎的性能；

	查询日志：
		log={ON|OFF}:是否记录所有语句的日志信息于一般查询日志文件(general_log);
		log_output={TABLE|FILE|NONE}
			TABLE和FILE可以同时出现，用逗号分隔即可；
		general_log：是否启用查询日志；
		general_log_file：定义一般查询日志保存的文件

	慢查询日志：
		long_query_time： 10.000000
		slow_query_log={ON|OFF}
			设定是否启用慢查询日志；它的输出位置也取决log_output={TABLE|FILE|NONE}；
		slow_query_log_file=www-slow.log 
			定义日志文件路径及名称；

		log_slow_filter=admin,filesort,filesort_on_disk,full_join,full_scan,query_cache,query_cache_miss,tmp_table,tmp_table_on_disk 
		log_slow_queries=ON                                                                                                           
		log_slow_rate_limit=1                                                                                                            
		log_slow_verbosity 

	错误日志：
		服务器启动和关闭过程中的信息；
		服务器运行过程中的错误信息；
		事件调度器运行一个事件时产生的信息；
		在复制架构中的从服务器上启动从服务器线程时产生的信息；

		log_error = /path/to/error_log_file
		log_warnings = {1|0}
			是否记录警告信息于错误日志中；

	二进制日志：
		时间点恢复
		复制

20140411
	回顾：
		日志文件：6类
			一般查询日志：log, general_log, log_output
			慢查询日志：
			错误日志
			二进制日志
			中继日志
			事务日志

	二进制日志：“修改”

	position：位置
	time: 时间

	滚动：
		1、大小
		2、时间

	二进制日志的功用：
		即时点恢复；
		复制；

	mysql> SHOW MASTER STATUS;
	mysql> FLUSH LOGS;
	mysql> SHOW BINARY LOGS;
	mysql> SHOW BINLOG EVENTS IN 'log_file';

	# mysqlbinlog 
		--start-time
		--stop-time
		--start-position
		--stop-position

	server-id: 服务器身份标识

	INSERT INTO t1 VALUE (CURRENT_DATE());

	MySQL记录二进制日志的格式：
		基于语句：statement
		基于行：row
		混合模式：mixed

	二进制日志文件内容格式：
		事件发生的日期和时间
		服务器ID
		事件的结束位置
		事件的类型
		原服务器生成此事件时的线程ID
		语句的时间戳和写入二进制日志文件的时间差；
		错误代码；
		事件内容
		事件位置，相当于下一事件的开始位置

	服务器参数：
		log_bin = {ON|OFF}, 还可以是个文件路径
		log_bin_trust_function_creators
		sql_log_bin = {ON|OFF}
		sync_binlog
		binlog_format = {statement|row|mixed}
		max_binlog_cache_size = 
			二进制日志缓冲空间大小，仅用于缓冲事务类的语句；
		max_binlog_stmt_cache_size =
		max_binlog_size = 
			二进制日志文件上限

	建议：切勿将二进制日志与数据文件放在一同设备；

中继日志：
	relay_log_purge={ON|OFF}
		是否自动清理不再需要中继日志




日志相关的服务器参数详解：


	expire_logs_days={0..99}
	设定二进制日志的过期天数，超出此天数的二进制日志文件将被自动删除。默认为0，表示不启用过期自动删除功能。如果启用此功能，自动删除工作通常发生在MySQL启动时或FLUSH日志时。作用范围为全局，可用于配置文件，属动态变量。

	general_log={ON|OFF}
	设定是否启用查询日志，默认值为取决于在启动mysqld时是否使用了--general_log选项。如若启用此项，其输出位置则由--log_output选项进行定义，如果log_output的值设定为NONE，即使用启用查询日志，其也不会记录任何日志信息。作用范围为全局，可用于配置文件，属动态变量。
	 
	general_log_file=FILE_NAME
	查询日志的日志文件名称，默认为“hostname.log"。作用范围为全局，可用于配置文件，属动态变量。


	binlog-format={ROW|STATEMENT|MIXED}
	指定二进制日志的类型，默认为STATEMENT。如果设定了二进制日志的格式，却没有启用二进制日志，则MySQL启动时会产生警告日志信息并记录于错误日志中。作用范围为全局或会话，可用于配置文件，且属于动态变量。

	log={YES|NO}
	是否启用记录所有语句的日志信息于一般查询日志(general query log)中，默认通常为OFF。MySQL 5.6已经弃用此选项。
	 
	log-bin={YES|NO}
	是否启用二进制日志，如果为mysqld设定了--log-bin选项，则其值为ON，否则则为OFF。其仅用于显示是否启用了二进制日志，并不反应log-bin的设定值。作用范围为全局级别，属非动态变量。
	 
	log_bin_trust_function_creators={TRUE|FALSE}
	此参数仅在启用二进制日志时有效，用于控制创建存储函数时如果会导致不安全的事件记录二进制日志条件下是否禁止创建存储函数。默认值为0，表示除非用户除了CREATE ROUTING或ALTER ROUTINE权限外还有SUPER权限，否则将禁止创建或修改存储函数，同时，还要求在创建函数时必需为之使用DETERMINISTIC属性，再不然就是附带READS SQL DATA或NO SQL属性。设置其值为1时则不启用这些限制。作用范围为全局级别，可用于配置文件，属动态变量。
	 
	log_error=/PATH/TO/ERROR_LOG_FILENAME
	定义错误日志文件。作用范围为全局或会话级别，可用于配置文件，属非动态变量。
	 
	log_output={TABLE|FILE|NONE}
	定义一般查询日志和慢查询日志的保存方式，可以是TABLE、FILE、NONE，也可以是TABLE及FILE的组合(用逗号隔开)，默认为TABLE。如果组合中出现了NONE，那么其它设定都将失效，同时，无论是否启用日志功能，也不会记录任何相关的日志信息。作用范围为全局级别，可用于配置文件，属动态变量。
	 
	log_query_not_using_indexes={ON|OFF}
	设定是否将没有使用索引的查询操作记录到慢查询日志。作用范围为全局级别，可用于配置文件，属动态变量。
	 
	log_slave_updates
	用于设定复制场景中的从服务器是否将从主服务器收到的更新操作记录进本机的二进制日志中。本参数设定的生效需要在从服务器上启用二进制日志功能。
	 
	log_slow_queries={YES|NO}
	是否记录慢查询日志。慢查询是指查询的执行时间超出long_query_time参数所设定时长的事件。MySQL 5.6将此参数修改为了slow_query_log。作用范围为全局级别，可用于配置文件，属动态变量。
	 
	log_warnings=#
	设定是否将警告信息记录进错误日志。默认设定为1，表示启用；可以将其设置为0以禁用；而其值为大于1的数值时表示将新发起连接时产生的“失败的连接”和“拒绝访问”类的错误信息也记录进错误日志。

	long_query_time=#
	设定区别慢查询与一般查询的语句执行时间长度。这里的语句执行时长为实际的执行时间，而非在CPU上的执行时长，因此，负载较重的服务器上更容易产生慢查询。其最小值为0，默认值为10，单位是秒钟。它也支持毫秒级的解析度。作用范围为全局或会话级别，可用于配置文件，属动态变量。

	max_binlog_cache_size{4096 .. 18446744073709547520}
	二进定日志缓存空间大小，5.5.9及以后的版本仅应用于事务缓存，其上限由max_binlog_stmt_cache_size决定。作用范围为全局级别，可用于配置文件，属动态变量。

	max_binlog_size={4096 .. 1073741824}
	设定二进制日志文件上限，单位为字节，最小值为4K，最大值为1G，默认为1G。某事务所产生的日志信息只能写入一个二进制日志文件，因此，实际上的二进制日志文件可能大于这个指定的上限。作用范围为全局级别，可用于配置文件，属动态变量。




	max_relay_log_size={4096..1073741824}
	设定从服务器上中继日志的体积上限，到达此限度时其会自动进行中继日志滚动。此参数值为0时，mysqld将使用max_binlog_size参数同时为二进制日志和中继日志设定日志文件体积上限。作用范围为全局级别，可用于配置文件，属动态变量。

	innodb_log_buffer_size={262144 .. 4294967295}
	设定InnoDB用于辅助完成日志文件写操作的日志缓冲区大小，单位是字节，默认为8MB。较大的事务可以借助于更大的日志缓冲区来避免在事务完成之前将日志缓冲区的数据写入日志文件，以减少I/O操作进而提升系统性能。因此，在有着较大事务的应用场景中，建议为此变量设定一个更大的值。作用范围为全局级别，可用于选项文件，属非动态变量。
	 
	innodb_log_file_size={108576 .. 4294967295}
	设定日志组中每个日志文件的大小，单位是字节，默认值是5MB。较为明智的取值范围是从1MB到缓存池体积的1/n，其中n表示日志组中日志文件的个数。日志文件越大，在缓存池中需要执行的检查点刷写操作就越少，这意味着所需的I/O操作也就越少，然而这也会导致较慢的故障恢复速度。作用范围为全局级别，可用于选项文件，属非动态变量。
	 
	innodb_log_files_in_group={2 .. 100}
	设定日志组中日志文件的个数。InnoDB以循环的方式使用这些日志文件。默认值为2。作用范围为全局级别，可用于选项文件，属非动态变量。
	 
	innodb_log_group_home_dir=/PATH/TO/DIR
	设定InnoDB重做日志文件的存储目录。在缺省使用InnoDB日志相关的所有变量时，其默认会在数据目录中创建两个大小为5MB的名为ib_logfile0和ib_logfile1的日志文件。作用范围为全局级别，可用于选项文件，属非动态变量。


	relay_log=file_name
	设定中继日志的文件名称，默认为host_name-relay-bin。也可以使用绝对路径，以指定非数据目录来存储中继日志。作用范围为全局级别，可用于选项文件，属非动态变量。

	relay_log_index=file_name
	设定中继日志的索引文件名，默认为为数据目录中的host_name-relay-bin.index。作用范围为全局级别，可用于选项文件，属非动态变量。

	relay-log-info-file=file_name
	设定中继服务用于记录中继信息的文件，默认为数据目录中的relay-log.info。作用范围为全局级别，可用于选项文件，属非动态变量。


	relay_log_purge={ON|OFF}
	设定对不再需要的中继日志是否自动进行清理。默认值为ON。作用范围为全局级别，可用于选项文件，属动态变量。

	relay_log_space_limit=#
	设定用于存储所有中继日志文件的可用空间大小。默认为0，表示不限定。最大值取决于系统平台位数。作用范围为全局级别，可用于选项文件，属非动态变量。


	slow_query_log={ON|OFF}
	设定是否启用慢查询日志。0或OFF表示禁用，1或ON表示启用。日志信息的输出位置取决于log_output变量的定义，如果其值为NONE，则即便slow_query_log为ON，也不会记录任何慢查询信息。作用范围为全局级别，可用于选项文件，属动态变量。

	slow_query_log_file=/PATH/TO/SOMEFILE
	设定慢查询日志文件的名称。默认为hostname-slow.log，但可以通过--slow_query_log_file选项修改。作用范围为全局级别，可用于选项文件，属动态变量。


	sql_log_bin={ON|OFF}
	用于控制二进制日志信息是否记录进日志文件。默认为ON，表示启用记录功能。用户可以在会话级别修改此变量的值，但其必须具有SUPER权限。作用范围为全局和会话级别，属动态变量。

	sql_log_off={ON|OFF}
	用于控制是否禁止将一般查询日志类信息记录进查询日志文件。默认为OFF，表示不禁止记录功能。用户可以在会话级别修改此变量的值，但其必须具有SUPER权限。作用范围为全局和会话级别，属动态变量。

	sync_binlog=#
	设定多久同步一次二进制日志至磁盘文件中，0表示不同步，任何正数值都表示对二进制每多少次写操作之后同步一次。当autocommit的值为1时，每条语句的执行都会引起二进制日志同步，否则，每个事务的提交会引起二进制日志同步。







备份和恢复：
	1、灾难恢复；
	2、审计；
	3、测试；

	备份：目的用于恢复；对备份数据做恢复测试；

	备份类型：

		根据备份时，数据库服务器是否在线：
			冷备：cold backup
			温备：warm backup
			热备：hot backup

		根据备份的数据集：
			完全备份：full backup
			部分备份: partial backup

		根据备份时的接口（直接备份数据文件还是通过mysql服务器导出数据）：
			物理备份：直接复制(归档)数据文件的备份方式；physical backup
			逻辑备份：把数据从库中提出出来保存为文本文件；logical backup
				mysqldump

		根据备份时是备份整个数据还是仅备份变化的数据：
			完全备份：full backup
			增量备份：incremental backup
			差异备份：differential backup

	备份策略：
		选择备份方式
		选择备份时间
		考虑到恢复成本
			恢复时长
		备份成本：
			锁时间
			备份时长
			备份负载

	备份对象：
		数据
		配置文件
		代码：存储过程，存储函数，触发器
		OS相关的配置文件，如crontab配置计划及相关的脚本

		跟复制相关的配置；
		二进制日志文件

	备份工具：
		mysqldump：逻辑备份工具
			InnoDB热备、MyISAM温备、Aria温备
			备份和恢复过程较慢
		mysqldumper: 多线程的mysqldump

			很难实现差异或增量备份；

		lvm-snapshot: 
			接近于热备的工具：因为要先请求全局锁，而后创建快照，并在创建快照完成后释放全局锁；
			使用cp、tar等工具进行物理备份；
			备份和恢复速度较快；

				很难实现增量备份，并且请求全局需要等待一段时间，在繁忙的服务器上尤其如此；

		SELECT clause INTO OUTFILE '/path/to/somefile'
		LOAD DATA INFILE '/path/from/somefile'
			部分备份工具， 不会备份关系定义，仅备份表中的数据；
			逻辑备份工具，快于mysqldump

		Innobase: 商业备份工具, innobackup
		Xtrabackup: 由Percona提供的开源备份工具
			InnoDB热备，增量备份；
			MyISAM温备，不支持增量；
			物理备份，速度快；

		mysqlhotcopy: 几乎冷备

	mysqldump: 
		mysqldump [options] [db_name [tbl_name ...]]

		备份单个库：mysqldump [options] db_name
			恢复时：如果目标库不存在，需要事先手动创建

		--all-databases:　备份所有库

		--databases db1 db2 ...: 备份指定的多个库

	注意：备份前要加锁

		--lock-all-tables：请求锁定所有表之后再备份，对MyISAM、InnoDB、Aria做温备

		--single-transaction: 能够对InnoDB存储引擎实现热备；

		备份代码：
			--events: 备份事件调度器代码
			--routines: 备份存储过程和存储函数
			--triggers：备份触发器

		备份时滚动日志：
			--flush-logs: 备份前、请求到锁之后滚动日志；

		复制时的同步位置标记：
			--master-data=[0|1|2]
				0: 不记录
				1：记录为CHANGE MASTER语句
				2：记录为注释的CHANGE MASTER语句

		使用mysqldump备份：
			请求锁：--lock-all-tables或使用--singe-transaction进行innodb热备；
			滚动日志：--flush-logs
			选定要备份的库：--databases
			记录二进制日志文件及位置：--master-data=

		恢复：
			建议：关闭二进制日志，关闭其它用户连接；

		备份策略：基于mysqldump
			备份：mysqldump+二进制日志文件；
				周日做一次完全备份：备份的同时滚动日志
				周一至周六：备份二进制日志；
			恢复：
				完全备份+各二进制日志文件中至此刻的事件

			对MySQL配置文件，以及与MySQL相关的OS配置文件在每次修改后都应该直接进行备份；

		练习：写一个备份脚本；
			1、备份所有数据库；
			2、要每周日凌晨自动执行；

	lvm-snapshot：基于LVM快照的备份
		1、事务日志跟数据文件必须在同一个卷上；
		2、创建快照卷之前，要请求MySQL的全局锁；在快照创建完成之后释放锁；
		3、请求全局锁完成之后，做一次日志滚动；做二进制日志文件及位置标记(手动进行)；

		备份步骤：
		1、请求全局锁，并滚动日志
		mysql> FLUSH TABLES WITH READ LOCK;
		mysql> FLUSH LOGS;

		2、做二进制日志文件及位置标记(手动进行)；
		# mysql -e 'show master status' > /path/to/somefile

		3、创建快照卷
		# lvcreate -L   -s -n    -p r  /path/to/some_lv

		4、释放全局锁
		mysql> UNLOCK TABLES;

		5、挂载快照卷并备份
		# cp

		6、备份完成之后，删除快照卷

		恢复：
		1、二进制日志保存好；
			提取备份之后的所有事件至某sql脚本中；
		2、还原数据，修改权限及属主属组等，并启动mysql
		3、做即时点还原

		mylvbackup: perl脚本，快速基于Lvm备份mysql

	xtrabackup: 

		回顾：
			mysqldump:
				--databases db1 db2 ...
				--all-databases
				--lock-all-tables
				--single-transaction
				--master-data={0|1|2}
				--flush-logs

		常用工具：mysqldump, lvm-snapshot, xtrabackup(percona)

			innobackupex: 需要MySQL服务处于运行状态

	注意：
		1、将数据和备份放在不同的磁盘设备上；异机或异地备份存储较为理想；
		2、备份的数据应该周期性地进行还原测试；
		3、每次灾难恢复后都应该立即做一次完全备份；
		4、针对不同规模或级别的数据量，要定制好备份策略；
		5、二进制日志应该跟数据文件在不同磁盘上，并周期性地备份好二进制日志文件；

	从备份中恢复应该遵循步骤：
		1、停止MySQL服务器；
		2、记录服务器的配置和文件权限；
		3、将数据从备份移到MySQL数据目录；其执行方式依赖于工具；
		4、改变配置和文件权限；
		5、以限制访问模式重启服务器；mysqld的--skip-networking选项可跳过网络功能；
			方法：编辑my.cnf配置文件，添加如下项：
			skip-networking
			socket=/tmp/mysql-recovery.sock
		6、载入逻辑备份（如果有）；而后检查和重放二进制日志；
		7、检查已经还原的数据；
		8、重新以完全访问模式重启服务器；
			注释前面在my.cnf中添加的选项，并重启；


	SELECT clause INTO OUTFILE ''
	LOAD DATA INFILE '' INTO TABLE tb_name


MySQL复制：

	扩展：
		scale on: 向上扩展，垂直扩展
		scale out：向外扩展，水平扩展

		1，4G：50 concurrent
		2*8=16, 32G , 300

	MySQL保存二进制日志：
		statement
		row
		mixed

		默认为异步工作模式

		SLAVE: 
			IO thread: 向主服务请求二进制日志中的事件
			SQL thread：从中继日志读取事件并在本地执行

		MASTER:
			binlog dump: 将IO thread请求的事件发送给对方；

		工作架构：
			从服务器：有且只能有一个主服务器；
				MariaDB-10：支持多主模型，多源复制(multi-source replication)
			一主多从：

		读写分离：主从模型下，让前端分发器能识别读/写，并且按需调度至目标主机；
			amoeba: 
			mysql-proxy：

		双主：master-master
			1、必须设定双方的自动增长属性，以避免冲突
				auto_increment_increment=#
					定义自动增长字段起始值
				auto_increment_offset=2
					步长
			2、数据不一致；
				Age, Salary

				A： update t1 set Salary=salary+1000 WHERE Age>=30;
				B:  update t1 set Age=Age-3 WHERE Salary < 3000;

			功能：均衡读请求；写请求双方一样；

		示例：主从复制的配置

			版本
				1、双方的MySQL要一致；
				2、如果不一致：主的要低于从的；

			从哪儿开始复制：
				1、都从0开始：
				2、主服务器已经运行一段时间，并且存在不小的数据集：
					把主服务器备份，然后在从服务恢复，从主服务器上备份时所处的位置开始复制；

		配置过程：
			主服务器：
				1、改server-id
				2、启用二进制日志
				3、创建有复制权限的帐号

			从服务器：
				1、改server-id
				2、启用中继日志
				3、连接主服务器
				4、启动复制线程

			连接主服务器的命令：
				CHANGE MASTER TO
					MASTER_HOST = '', MASTER_USER='', MASTER_PASSWORD='', MASTER_LOG_FILE='', MASTER_LOG_POS=;


20140412

回顾：
	主：binlog dump
	从：io thread, sql thread

	传统概念：
		1、一主可以多从
		2、一从只能有一主（单源复制）
	新机制：
		3、一从多主（多源复制），MariaDB-10

	主：read/write
	从：read

	简单的主从复制：
		一、master
			1、启用二进制日志
				log-bin=/mydata/binlogs/master-bin
				log-bin-index=

			2、为master选择一个在当前复制架构中惟一的server-id
				server-id={0-2^32}

			3、创建一个具有复制权限的用户帐号
				mysql> GRANT REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'repluser'@'hostname' IDENTIFIED BY 'repluser_pass';
				mysql> FLUSH PRIVILEGES;

		二、slave
			1、启用中继日志（并关闭二进制日志）
				relay-log=/mydata/relaylogs/relay-bin
				relay-log-index=

			2、为slave选择一个在当前复制架构中惟一的server-id
				server-id={0-2^32}

			3、连接至主服务器
				mysql> CHANGE MASTER TO MASTER_HOST='', MASTER_USER='', MASTER_PASSWORD='', MASTER_LOG_FILE='', MASTER_LOG_POS=#;
				mysql> START SLAVE;

	MySQL简单复制应用扩展：
		1、主从服务器时间要同步（ntp）：
			*/5 * * * * /usr/sbin/ntpdate 172.16.0.1

		2、如何限制从服务器只读？
			read-only=ON

			注意：仅能限制那不具有SUPER权限用户无法执行写操作；

			想限制所有用户：
			mysql> FLUSH TABLES WITH READ LOCK;

		3、如何主从复制时的事务安全？
			在主服务器上配置：
			sync_binlog=1

		4、半同步复制
			主服务器：
				mysql> INSTALL PLUGIN rpl_semi_sync_master SONAME 'semisync_master.so';
				mysql> SHOW GLOBAL VARIABLES LIKE '%semi%';
				mysql> SET GLOBAL rpl_semi_sync_master_enabled=ON;
				mysql> SET GLOBAL rpl_semi_sync_master_timeout=1000;

			从服务器：
				mysql> INSTALL PLUGIN rpl_semi_sync_slave SONAME 'semisync_slave.so';
				mysql> SET GLOBAL rpl_semi_sync_slave_enabled=ON;
				mysql> STOP SLAVE IO_THREAD; START SLAVE IO_THREAD;

			在主服务器验正半同步复制是否生效：
				mysql> SHOW GLOBAL STATUS LIKE '%semi%';

			一旦某次等待超时，会自动降级为异步；

		5、复制过滤器
			master: 
				binlog_do_db=
				binlog_ignore_db=

			slave:
				replicate_do_db=
				replicate_ignore_db=

				replicate_do_table= db_name.table_name
				replicate_ignore_table=

				replicate_wild_do_table=
				replicate_wild_ignore_table=

			my.cnf: [mysqld]


		6、多主模型

			1）、在两台服务器上各自建立一个具有复制权限的用户；
			2）、修改配置文件：
			# 主服务器A上
			[mysqld]
			server-id = 10
			log-bin = mysql-bin
			relay-log = relay-mysql
			auto-increment-offset = 1
			auto-increment-increment = 2


			# 主服务器B上
			[mysqld]
			server-id = 20
			log-bin = mysql-bin
			relay-log = relay-mysql
			auto-increment-increment = 2
			# 步长
			auto-increment-offset = 2 
			# 起始值

			3）、如果此时两台服务器均为新建立，且无其它写入操作，各服务器只需记录当前自己二进制日志文件及事件位置，以之作为另外的服务器复制起始位置即可

			serverA|mysql> SHOW MASTER STATUS\G
			************************** 1. row ***************************
			            File: mysql-bin.000001
			        Position: 710
			    Binlog_Do_DB: 
			Binlog_Ignore_DB: 
			1 row in set (0.00 sec)

			server2|mysql> SHOW MASTER STATUS\G
			mysql> SHOW MASTER STATUS\G
			*************************** 1. row ***************************
			            File: mysql-bin.000003
			        Position: 811
			    Binlog_Do_DB: 
			Binlog_Ignore_DB: 
			1 row in set (0.00 sec)

			4、各服务器接下来指定对另一台服务器为自己的主服务器即可：
			serverA|mysql> CHANGE MASTER TO ...,MASTER_LOG_FILE='mysql-bin.000003', MASTER_LOG_POS=811

			serverB|mysql> CHANGE MASTER TO ...,MASTER_LOG_FILE='mysql-bin.000001', MASTER_LOG_POS=710	


			多主，且高可用的解决方案：
				MMM：Multi Master MySQL
				MHA：MySQL HA

			独自完成：MMM

		7、SSL
			独自完成：基于ssl的复制	

	博客作业：
		1、上述1-7步；
		2、MMM

		主从：

			从多个服务器，以均衡为目的挑选一个响应客户端请求的服务器：
				round-robin: 
				取模


			memcached: 缓存能力+API
			读写分离：读写分离，负载均衡


	复制相关的文件：
		master.info: 文本文件，保存从服务器连接至主服务时所需要的信息，每行一个值；
		relay-log.info: 文本文件，保存了复制位置：包括二进制日志和中继日志的文件及位置；

		为了复制的安全性：
			sync_master_info = 1    
			sync_relay_log = 1   
			sync_relay_log_info = 1

		从服务器意外崩溃时，建议使用pt-slave-start命令来启动slave; 

	基于行和基于语句复制：
		基于语句：
			数据量小；易于查看；适应性强；
			有些语句无法做精确复制；无法对使用了触发器、存储过程等代码的应用实现精确复制；

		基于行：
			能够精确完成有着触发器、存储过程等代码场景中的复制；能完成几乎所有的复制功能；较少的CPU占用率；
			无法判断执行了什么样的SQL语句；数据量可能略大；

	从服务器落后于主服务器：
		Seconds_Behind_Master: 0

	评估主从服务表中的数据是否一致：
		pt-table-checksum

		如果数据不一致，解决办法
			1、重新备份并在从服务器导入数据；
			2、pt-table-sync 

		为了提高复制时的数据安全性，在主服务器上的设定：
			sync_binlog = 1
			innodb_flush_log_at_trx_commit = 1
				此参数的值设定为1，性能下降会较严重；因此，一般设定为2等，此时，主服务器崩溃依然有可能导致从服务器无法获取到全部的二进制日志事件；

		如果master意外崩溃导致二进制日志中的某事件损坏，可以在从服务器使用如下参数忽略：
			sql_slave_skip_counter = 0


	第三方复制解决方案：Tungsten, Galera 


	MariaDB GTID:

		文档中应用MariaDB-10，需要做的修改：
		1、不支持的参数：
			gtid-mode=on 
			enforce-gtid-consistency=true

		2、修改的参数：
			slave-parallel-workers参数修改为slave-parallel-threads

		3、连接至主服务使用的命令：
			一个新的参数：MASTER_USER_GTID={current_pos|slave_pos|no}

			CHANGE MASTER TO master_host="127.0.0.1", master_port=3310, master_user="root", master_use_gtid=current_pos;

	Multi-Source Replication: 
		CHANGE MASTER ['connection_name'] ...
		FLUSH RELAY LOGS ['connection_name']
		MASTER_POS_WAIT(....,['connection_name'])
		RESET SLAVE ['connection_name']
		SHOW RELAYLOG ['connection_name'] EVENTS
		SHOW SLAVE ['connection_name'] STATUS
		SHOW ALL SLAVES STATUS
		START SLAVE ['connection_name'...]]
		START ALL SLAVES ...
		STOP SLAVE ['connection_name'] ...
		STOP ALL SLAVES ...

		总结：多源复制，每个源应该使用不同的数据库；多源复制目前不支持半同步复制；

	总结：GTID（HA，多线程复制）、多源复制


忘记管理员密码的解决方式：
	启动mysqld时，使用--skip-grant-tables 和 --skip-networking


tcpdump -i any -s 0 -A -n -p port 3306 and src IP|grep -i -E 'SELECT|INSERT'

协议报文分析器：
	sniffer: 商业工具

tcpdump, wireshark(GUI), tshark(CLI)

tcpdump [options] 过滤条件

获取报文的条件：		
		
ip src host 172.16.100.1
tcp src or dst port 21

udp dst port 53

tcp src or dst port 21 AND src host 172.16.100.1

tcp port 21 AND host 172.16.100.1
		

协议：
	ip, tcp, udp

流向：
	src, dst, src and dst, src or dst

对象:
	net, port, host
		src net 192.168.0.0/24
		src host 192.168.10.16
		dst port 3306 

多条件：
	and, or, !


tcpdump -i eth0 -A -nn -s0 tcp dst port 3306 and ip dst host 192.168.10.16		

tcpdump的语法：
tcpdump [options] [Protocol] [Direction] [Host(s)] [Value] [Logical Operations] [Other expression]

Protocol(协议):
Values(取值): ether, fddi, ip, arp, rarp, decnet, lat, sca, moprc, mopdl, tcp and udp.
If no protocol is specified, all the protocols are used. 

Direction(流向):
Values(取值): src, dst, src and dst, src or dst
If no source or destination is specified, the "src or dst" keywords are applied. (默认是src or dst)
For example, "host 10.2.2.2" is equivalent to "src or dst host 10.2.2.2".


Host(s)(主机):
Values(替代关键字): net, port, host, portrange.
If no host(s) is specified, the "host" keyword is used. 默认如果此段没有指定关键字，默认即host。
For example, "src 10.1.1.1" is equivalent to "src host 10.1.1.1". 


Logical Operations:
(1) AND 
and or &&
(2) OR 
or or ||
(3) EXCEPT 
not or !


常用选项：

-i any : Listen on all interfaces just to see if you're seeing any traffic.
-n : Don't resolve hostnames.
-nn : Don't resolve hostnames or port names.
-X : Show the packet's contents in both hex and ASCII.
-XX : Same as -X, but also shows the ethernet header.
-v, -vv, -vvv : Increase the amount of packet information you get back.
-c # : Only get x number of packets and then stop.
-s : Define the snaplength (size) of the capture in bytes. Use -s0 to get everything, unless you are intentionally capturing less.
-S : Print absolute sequence numbers.
-e : Get the ethernet header as well.
-q : Show less protocol information.
-E : Decrypt IPSEC traffic by providing an encryption key.
-A ：Display Captured Packets in ASCII
-w /path/to/some_file : Capture the packets and write into a file 
-r /path/from/some_file : Reading the packets from a saved file 
-tttt : Capture packets with proper readable timestamp


ip host 172.16.100.1
ip src host 172.16.100.1
ip dst host 172.16.100.1
ip src and dst host 172.16.100.1

tcp src port 110

tcpdump -i eth0 -s0 -nn -XX tcp dst port 3306 and dst host 192.168.10.16 

Mroonga（http://mroonga.org/）
