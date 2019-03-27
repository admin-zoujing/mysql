# mysql
mysql for 5.7


查看正在连接的线程: show full processlist;
查看当前打开的表：show open tables;
查看服务器状态: show status like '%lock%';
查看innodb引擎的运行时信息: show engine innodb status\G;
查看服务器配置参数: show variables like '%timeout%';

创建用户：
mysql> CREATE USER 'springdev'@'localhost' IDENTIFIED BY 'springdev_mysql';
mysql>create database springdev  default charset 'utf8mb4';
授权test用户拥有testDB数据库的所有权限（某个数据库的所有权限）：
mysql>grant all privileges on springdev.* to springdev@localhost identified by 'springdev_mysql';
mysql>flush privileges;

mysql导入导出sql文件
window下
1.导出整个数据库
mysqldump -u 用户名 -p 数据库名 > 导出的文件名
mysqldump -u dbuser -p dbname > dbname.sql
2.导出一个表
mysqldump -u 用户名 -p 数据库名 表名> 导出的文件名
mysqldump -u dbuser -p dbname users> dbname_users.sql
3.导出一个数据库结构
mysqldump -u dbuser -p -d --add-drop-table dbname >d:/dbname_db.sql
-d 没有数据 --add-drop-table 在每个create语句之前增加一个drop table
4.导入数据库
常用source 命令
进入mysql数据库控制台，如
mysql -u root -p
mysql>use 数据库
然后使用source命令，后面参数为脚本文件(如这里用到的.sql)
mysql>source d:/dbname.sql
 

1. 导入数据到数据库
mysql -uroot -D数据库名 
1. 导入数据到数据库中得某个表
mysql -uroot -D数据库名  表名

D:\APMServ5.2.6\MySQL5.1\bin>mysqldump -u root -p  erp lightinthebox_tags > ligh
tinthebox.sql

 

linux下
一、导出数据库用mysqldump命令（注意mysql的安装路径，即此命令的路径）：
1、导出数据和表结构：
mysqldump -u用户名 -p密码 数据库名 > 数据库名.sql
#/usr/local/mysql/bin/   mysqldump -uroot -p abc > abc.sql
敲回车后会提示输入密码
2、只导出表结构
mysqldump -u用户名 -p密码 -d 数据库名 > 数据库名.sql
#/usr/local/mysql/bin/   mysqldump -uroot -p -d abc > abc.sql
注：/usr/local/mysql/bin/  --->  mysql的data目录


二、导入数据库
1、首先建空数据库
mysql>create database abc  default charset 'utf8mb4';

2、导入数据库
方法一：
（1）选择数据库
mysql>use abc;
（2）设置数据库编码
mysql>set names utfmb4;
（3）导入数据（注意sql文件的路径）
mysql>source /home/abc/abc.sql;
方法二：
mysql -u用户名 -p密码 数据库名 < 数据库名.sql
#mysql -uabc_f -p abc < abc.sql


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
		
