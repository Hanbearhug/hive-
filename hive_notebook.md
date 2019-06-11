# 基础命令
```
show databases; # 查看某个数据库
use 数据库; # 进入某个数据库
desc 表名; # 显示表结构
show partitions 表名; # 显示表名的分区
show create table_name; # 显示创建表的结构


# 建表语句
# 内部表
use xxdb; create table xxx;
# 创建一个表，结构与其他一样
create table xxx like xxx;
# 外部表
use xxdb; create external table xxx;
# 分区表
use xxdb; create external table xxx(I int) partitioned by (d string)
# 内外部表转化
alter table table_name set TBLPROPROTIES('EXTERNAL'='TRUE'); # 内部表转外部表
alter table table_name set TBLPROPROTIES('EXTERNAL'='FALSE'); # 外部表转内部表

# 表结构修改
# 重命名表
use xxxdb; alter table table_name rename to new_table_name;
# 增加字段
alter table table_name add columns(newcol1 int comment'新增');
# 删除表
use xxxdb; drop table table_name;
# 删除分区
# 注意：若是外部表，则还需要删除文件(hadoop fs -rm -r -f hdfspath)
alter table table_name drop if exists partitions(d='2016-07-01');

# 字段类型
# tinyint, smallint, int, bigint, float, decimal, boolean, string
# 复合数据类型
# struct, array, map
```
