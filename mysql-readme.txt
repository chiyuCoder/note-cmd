-D，--database=name 打开指定数据库
--delimiter = name 指定分隔符
-h,  --hostname=name 服务器名称
-p, --password[=name] 密码
-P, --port=# 端口号
--prompt=name 设置提示符
-u, --user=name 用户名
-V，--version 输出版本信息并退出

exit;（要加上分号）
quit;（要加上分号）
\q 退mysq出（三选一）

mysql提示符: 
	\D 完整日期
	\d 当前数据库
	\h 服务器名称
	\u 当前用户


mysql常用命令:
	显示当前服务器版本：SELECT VERSION();
	显示当前日期时间： SELECT NOW();
	显示当前用户： SELECT USER();
mysql语句规范
	关键字与函数名称全部大写
	数据库名称、表名称、字段名称全部小写
	SQL语句必须以分号结尾


/*{}表示至少选一项， []表示可不填写,|表示‘或者’*/
创建数据库: CREATE {DATABASE | SCHEMA} [IF NOT EXISTS] db_name [[DEFAULT] CHARACTER SET [=] charset_name];
查看当前服务器下的数据库列表：SHOW {DATABASES | SCHEMAS} [LIKE 'pattern' | WHERE expr];
查看警告信息：SHOW WARNINGS;
查看数据库的默认设置：SHOW CREATE DATABASE db_name;
修改数据库：ALTER {DATABASE | SCHEMA} [db_name] [DEFAULT] CHARACTER SET [=] charaset_name;
删除数据库： DROP {DATABASE | SCHEMA} [IF EXISTS] db_name;


打开数据库: USE 数据库名称；
显示当前打开的数据库：SELECT DATABASE();
创建数据表： CREATE TABLE [IF NOT EXISTS] table_name (
	column_name data_type,
	column_name data_type NOT NULL AUTO_INCREMENT PRIMARY KEY UNIQUE KEY  DEFAULT default_text,
	FOREIGN KEY （column_name）REFERENCES other_table_name (other_column_name); 
/*AUTO_INCREMENT 自动编号（从1开始，必须与主键配合使用）， PRIMARY KEY 主键(不为空值),UNIQUE KEY（可有一个空值）,DEFAULT 如果没有值即为默认值,FOREIGN KEY为外链约束*/
	...
);
查看数据表：SHOW TABLES [FROM db_name] [LIKE 'pattern' | WHERE expr];
删除数据表：DROP TABLE  [IF EXISTS] table_name;
查看数据表列表： SHOW COLUMNS FROM table_name;
插入记录： INSERT [INTO] table_name [(col_name,...)] VALUES(value,...);
记录查找： SELECT expression,... FROM table_name;(*示全部)
删除记录 DELETE FROM table_name WHERE expr
查看索引：SHOW INDEX FROM table_name

外键约束的参照操作
1.CASCADE: 从父表删除或更新记录且自动删除或更新子表匹配的记录；
2.SET NULL：从父表删除或更新记录，并设置子表中的外键列为NULL。如果使用该选项，必须保证子表列没有指定NOT NULL；
3.RESTRICT：拒绝对父表的删除或更新操作。
4.NO ACTION:在mysql中和RESTRICT相同。
e.g.: 
...
...
	FOREIGN KEY (colname1) REFERENCES tbl(colname2) ON DELETE CASCADE
...
..


修改数据表：
	添加单列：ALTER TABLE table_name ADD [COLUMN] column_name column_definition [FIRST | AFTER col_name];
	添加多列： ALTER TABLE table_name ADD [COLUMN] (col_name column_definition, ...)
	删除列： ALTER TABLE table_name DROP [COLUMN] col_name [, DROP ....];
	添加主键约束：ALTER TABLE table_name [CONSTRAINT [symbol]] PRIMARY KEY [index_type] (index_col_name, ...);
	添加唯一约束： ALTER TABLE table_name ADD[CONSTRAINT [symbol]] UNIQUE [INDEX | KEY] [index_name] [index_type] (index_col_name, ...);
	添加外键约束 ALTER TABLE table_name ADD [CONSTRAINT [symbol]] FOREIGN KEY [index_name] (index_col_name,...) reference_definition;
	添加/删除默认约束: ALTER TABLE table_name ALTER [COLUMN] col_name {SET DEFAULT default_text| DROP DEFAULT};
	删除主键约束：ALTER TABLE table_name DROP PRIMARY KEY;
	删除唯一约束：ALTER TABLE table_name DROP {INDEX | KEY} index_name;
	删除外键约束：ALTER TABLE table_name DROP FOREIGN KEY fk_symbol;
	修改列定义： ALTER TABLE table_name MODIFY [COLUMN] col_name column_definition [FIRST | AFTER col_name];
	修改列名称： ALTER TABLE table_name CHANGE [COLUMN] old_col_name new_col_name column_definition [FIRST | AFTER col_name];
	修改数据表名称：
		方法1： ALTER TABLE table_name RENAME [TO|AS] new_table_name
		方法2： RENAME TABLE table_name TO new_table_name[, table_name2 TO new_table_name2,...];

修改记录：
	插入记录： 
		方法1：INSERT [INTO] table_name [(col_name,....)] {VALUES | VALUE} ({expression | DEFAULT},...),(...),... 
		方法2： INSERT [INTO] table_name SET col_name={expression | DEFAULT},... 
		方法3：INSERT [INTO] table_name (col_name,...) SELECT ...;
	更新记录：(单表)UPDATE [LOW_PRIORITY] [IGNORE] table_reference SET col_name1={expr1|DEFAULT}[,col_name2={expr2|DEFAULT}...] [WHERE where_condition];
		：（多表）UPDATE table_references SET col_name1={expr1 | DEFAULT}
		[,col_name2={expr2|DEFAULT},...][WHERE where_condition];
/*mysql> UPDATE tdb_goods INNER JOIN tdb_goods_cates ON goods_cate = cate_name
    -> SET goods_cate = cate_id;*/
		:(多表2) CREATE TABLE [IF NOT EXISTS] table_name [(create_definition,...)] select_statement;
	删除记录：（单表）：
		DELETE FROM table_name  [WHERE where_condition];
	查找记录：
		SELECT select_expr [, select_expr,...][
			FROM table_references
			[WHERE where_condition]
			[GROUP BY {col_name | position} [ASC | DESC],...] 
			[HAVING where_condition]
			[ORDER BY {col_name | expr | position}[ASC | DESC],...]
			[LIMIT {[offset,] row_count | row_count OFFSET offset}]
/*LIMIT 2,2(out = id3,4)*/
		];


	连接：
	语法结构：
	table_reference {[INNER | CROSS] JOIN | {LEFT | RIGHT} [OUTER] JOIN} table_reference ON conditional_expr;
	
	