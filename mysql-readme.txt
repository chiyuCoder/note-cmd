-D��--database=name ��ָ�����ݿ�
--delimiter = name ָ���ָ���
-h,  --hostname=name ����������
-p, --password[=name] ����
-P, --port=# �˿ں�
--prompt=name ������ʾ��
-u, --user=name �û���
-V��--version ����汾��Ϣ���˳�

exit;��Ҫ���Ϸֺţ�
quit;��Ҫ���Ϸֺţ�
\q ��mysq������ѡһ��

mysql��ʾ��: 
	\D ��������
	\d ��ǰ���ݿ�
	\h ����������
	\u ��ǰ�û�


mysql��������:
	��ʾ��ǰ�������汾��SELECT VERSION();
	��ʾ��ǰ����ʱ�䣺 SELECT NOW();
	��ʾ��ǰ�û��� SELECT USER();
mysql���淶
	�ؼ����뺯������ȫ����д
	���ݿ����ơ������ơ��ֶ�����ȫ��Сд
	SQL�������ԷֺŽ�β


/*{}��ʾ����ѡһ� []��ʾ�ɲ���д,|��ʾ�����ߡ�*/
�������ݿ�: CREATE {DATABASE | SCHEMA} [IF NOT EXISTS] db_name [[DEFAULT] CHARACTER SET [=] charset_name];
�鿴��ǰ�������µ����ݿ��б�SHOW {DATABASES | SCHEMAS} [LIKE 'pattern' | WHERE expr];
�鿴������Ϣ��SHOW WARNINGS;
�鿴���ݿ��Ĭ�����ã�SHOW CREATE DATABASE db_name;
�޸����ݿ⣺ALTER {DATABASE | SCHEMA} [db_name] [DEFAULT] CHARACTER SET [=] charaset_name;
ɾ�����ݿ⣺ DROP {DATABASE | SCHEMA} [IF EXISTS] db_name;


�����ݿ�: USE ���ݿ����ƣ�
��ʾ��ǰ�򿪵����ݿ⣺SELECT DATABASE();
�������ݱ� CREATE TABLE [IF NOT EXISTS] table_name (
	column_name data_type,
	column_name data_type NOT NULL AUTO_INCREMENT PRIMARY KEY UNIQUE KEY  DEFAULT default_text,
	FOREIGN KEY ��column_name��REFERENCES other_table_name (other_column_name); 
/*AUTO_INCREMENT �Զ���ţ���1��ʼ���������������ʹ�ã��� PRIMARY KEY ����(��Ϊ��ֵ),UNIQUE KEY������һ����ֵ��,DEFAULT ���û��ֵ��ΪĬ��ֵ,FOREIGN KEYΪ����Լ��*/
	...
);
�鿴���ݱ�SHOW TABLES [FROM db_name] [LIKE 'pattern' | WHERE expr];
ɾ�����ݱ�DROP TABLE  [IF EXISTS] table_name;
�鿴���ݱ��б� SHOW COLUMNS FROM table_name;
�����¼�� INSERT [INTO] table_name [(col_name,...)] VALUES(value,...);
��¼���ң� SELECT expression,... FROM table_name;(*ʾȫ��)
ɾ����¼ DELETE FROM table_name WHERE expr
�鿴������SHOW INDEX FROM table_name

���Լ���Ĳ��ղ���
1.CASCADE: �Ӹ���ɾ������¼�¼���Զ�ɾ��������ӱ�ƥ��ļ�¼��
2.SET NULL���Ӹ���ɾ������¼�¼���������ӱ��е������ΪNULL�����ʹ�ø�ѡ����뱣֤�ӱ���û��ָ��NOT NULL��
3.RESTRICT���ܾ��Ը����ɾ������²�����
4.NO ACTION:��mysql�к�RESTRICT��ͬ��
e.g.: 
...
...
	FOREIGN KEY (colname1) REFERENCES tbl(colname2) ON DELETE CASCADE
...
..


�޸����ݱ�
	��ӵ��У�ALTER TABLE table_name ADD [COLUMN] column_name column_definition [FIRST | AFTER col_name];
	��Ӷ��У� ALTER TABLE table_name ADD [COLUMN] (col_name column_definition, ...)
	ɾ���У� ALTER TABLE table_name DROP [COLUMN] col_name [, DROP ....];
	�������Լ����ALTER TABLE table_name [CONSTRAINT [symbol]] PRIMARY KEY [index_type] (index_col_name, ...);
	���ΨһԼ���� ALTER TABLE table_name ADD[CONSTRAINT [symbol]] UNIQUE [INDEX | KEY] [index_name] [index_type] (index_col_name, ...);
	������Լ�� ALTER TABLE table_name ADD [CONSTRAINT [symbol]] FOREIGN KEY [index_name] (index_col_name,...) reference_definition;
	���/ɾ��Ĭ��Լ��: ALTER TABLE table_name ALTER [COLUMN] col_name {SET DEFAULT default_text| DROP DEFAULT};
	ɾ������Լ����ALTER TABLE table_name DROP PRIMARY KEY;
	ɾ��ΨһԼ����ALTER TABLE table_name DROP {INDEX | KEY} index_name;
	ɾ�����Լ����ALTER TABLE table_name DROP FOREIGN KEY fk_symbol;
	�޸��ж��壺 ALTER TABLE table_name MODIFY [COLUMN] col_name column_definition [FIRST | AFTER col_name];
	�޸������ƣ� ALTER TABLE table_name CHANGE [COLUMN] old_col_name new_col_name column_definition [FIRST | AFTER col_name];
	�޸����ݱ����ƣ�
		����1�� ALTER TABLE table_name RENAME [TO|AS] new_table_name
		����2�� RENAME TABLE table_name TO new_table_name[, table_name2 TO new_table_name2,...];

�޸ļ�¼��
	�����¼�� 
		����1��INSERT [INTO] table_name [(col_name,....)] {VALUES | VALUE} ({expression | DEFAULT},...),(...),... 
		����2�� INSERT [INTO] table_name SET col_name={expression | DEFAULT},... 
		����3��INSERT [INTO] table_name (col_name,...) SELECT ...;
	���¼�¼��(����)UPDATE [LOW_PRIORITY] [IGNORE] table_reference SET col_name1={expr1|DEFAULT}[,col_name2={expr2|DEFAULT}...] [WHERE where_condition];
		�������UPDATE table_references SET col_name1={expr1 | DEFAULT}
		[,col_name2={expr2|DEFAULT},...][WHERE where_condition];
/*mysql> UPDATE tdb_goods INNER JOIN tdb_goods_cates ON goods_cate = cate_name
    -> SET goods_cate = cate_id;*/
		:(���2) CREATE TABLE [IF NOT EXISTS] table_name [(create_definition,...)] select_statement;
	ɾ����¼����������
		DELETE FROM table_name  [WHERE where_condition];
	���Ҽ�¼��
		SELECT select_expr [, select_expr,...][
			FROM table_references
			[WHERE where_condition]
			[GROUP BY {col_name | position} [ASC | DESC],...] 
			[HAVING where_condition]
			[ORDER BY {col_name | expr | position}[ASC | DESC],...]
			[LIMIT {[offset,] row_count | row_count OFFSET offset}]
/*LIMIT 2,2(out = id3,4)*/
		];


	���ӣ�
	�﷨�ṹ��
	table_reference {[INNER | CROSS] JOIN | {LEFT | RIGHT} [OUTER] JOIN} table_reference ON conditional_expr;
	
	