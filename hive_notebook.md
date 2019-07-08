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
# 复合数据类型
```
# array
create table person(name string,work_locations array<string>)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ',';
# 数据
biansutao beijing,shanghai,tianjin,hangzhou
linan changchu,chengdu,wuhan
# 入库数据
LOAD DATA LOCAL INPATH '/home/hadoop/person.txt' OVERWRITE INTO TABLE person;
select * from person;
# biansutao ["beijing","shanghai","tianjin","hangzhou"]
# linan ["changchu","chengdu","wuhan"]

# map
create table score(name string,score map<string,int>)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY '\t'
COLLECTION ITEMS TERMINATED BY ','
MAP KEYS TERMINATED BY ':';
# 数据
biansutao '数学':80,'语文':89,'英语':95
jobs '语文':60,'数学':80,'英语':99
# 入库数据
LOAD DATA LOCAL INPATH '/home/hadoop/score.txt' OVERWRITE INTO TABLE score;
# biansutao {"数学":80,"语文":89,"英语":95}
# jobs {"语文":60,"数学":80,"英语":99}
  
# struct
CREATE TABLE test(id int,course struct<course:string,score:int>)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ',';
# 数据
1 english,80
2 math,89
3 chinese,95
# 入库
LOAD DATA LOCAL INPATH '/home/hadoop/test.txt' OVERWRITE INTO TABLE test;
# 查询
select * from test;
# 1  {"course":"english","score":80}
# 2  {"course":"math","score":89}
# 3  {"course":"chinese","score":95}
```

# 配置优化
```
# 开启任务并行执行
set hive.exec.parallel=true
# 设置运行内存
set mapreduce.map.memory.mb=1024;
set mapreduce.reduce.memory.mb=1024;
# 指定队列
set mapreduce.job.queuename=jppkg_high;
# 动态分区，为了防止一个reduce处理写入一个分区导致速度严重降低，下面需设置为false
# 默认为true

# 稍微复杂的用法
```
# 对json格式解析并分别命名列名，使用lateral view+json_tuple实现
select analysis_josn.* from
main_table lateral view json_tuple(main_table,'key1','key2','key3') analysis_json
as key1,key2,key3
```
# 动态分区，为了防止一个reduce处理写入一个分区导致速度严重降低，下面需设置为falsejosn
# 动态分区，为了防止一个reduce处理写入一个分区导致速度严重降低，下面需设置为false
