####  Explain的作用 ####
用于查看来自优化器执行SQL的信息。

#### 执行计划的字段信息 #### 

* id

表示查询中select子句或操作表的执行顺序。值越大，优先级越高，先被执行。
当sql包含子查询时，嵌套的最里面的子查询id越大，最先被执行。

* select_type

表示select查询的类型。主要用于区分各种复杂的查询。

  1. SIMPLE：表示最简单的select查询语句。也就是在查询中不包含子查询或union等并集差集操作。
  2. PRIMARY: 当查询语句包含任何复杂的子部分，则最外层的查询标记为PRIMARY。
  3. SUBQUERY: 当select或where列表出现子查询。那么该子查询标记为SUBQUERY。
  4. DERIVED: 英文是导出、派生。在from列表中包含的子查询标记为DERIVED。
  5. UNION： union后面出现的select查询，被标记为UNION。

* table
要查询的表名。并不一定是真实的表名。可以是别名、临时表。

* partitions
查询时匹配到的分区信息。对于非分区表显示NULL。当查询到的是分区表时，显示分区的命中信息。

* type

查询的类型。SQL优化的一个重要指标。性能从好到坏是
system > const > eq_ref > ref > ref_or_null > index_merge > unqiue_subquery > 
index_subquery > range > index > ALL
   1. system: 当表中仅有一行记录时（系统表），数据量很少，不需要磁盘IO。
   2. const：表示查询命中主键或唯一索引，且连接部分是一个常量值。 where id = 1;
   3. eq_ref: 在连表查询下，命中主键或唯一索引。其实就是被连接的部分是一个引用。
    where a.id = b.a_id;
   4. ref: 区别于eq_ref。查询命中非唯一索引。会找到很多个符合条件的行。 
   ````
   where a.name = "xin" // name 字段建立普通索引
   ````
   5. ref_or_null:  类似于ref。查询命中非唯一索引，但是NULL值也包括在内。
   ````
   where a.name = "xin" OR a.name IS NULL  // name 字段建立普通索引
   ````
   6. index_merge: 使用了索引合并优化的方法，查询使用了两个以上的索引。
   7. unique_subquery: 子查询中使用了唯一索引（也包括主键索引）。
   8. index_subquery:  区别于unique_subquery。子查询使用了非唯一索引。
   9. range: 针对一个有索引的字段，给定范围检索数据。在where子句中使用between...and,<,>,in 等条件查询都属于range。
   10. index: index和ALL 都是读全表，但index是读索引，而ALL是读取磁盘。
   11. ALL： 遍历全表。

* possible_keys:

在查询中，可能 使用到的索引。

* keys

区别于possible_keys. 查询中，实际使用到的索引。当type=index_merge时，可能会显示多个索引。

* key_len

表示查询中实际使用到的索引长度（单位字节）。原则上越短越好。 

包含多列索引的情况下，也只计算实际使用到的索引。

key_len 只计算where子句中的索引长度。排序和分组不计算。

* ref

查询的引用。常见的有：const、func、null、字段名
  1. 当使用常量等值查询时，显示const。
  2. 当关联查询时，会显示关联的字段名。
  3. 当查询条件使用了表达式、函数、或条件列发生了隐式转换。可能显示 func。
  4. 其他情况下显示null
  
* rows

一个估算的值。找到所需的记录，可能要扫描的行数。一般情况下，值越小越好。

* filtered

一个百分比值。存储引擎返回的数据定义为 A。A经过条件过滤后的数据定义为B。 那么 B/A 。

* extra

存储额外的信息。
 
  1. Using Index
  
  使用了索引覆盖。即要查询的数据通过索引即可返回。不必通过二级索引，查到主键后再通过主键获取数据。
  select的字段都建立了索引的情况下，会产生索引覆盖。
  
  2. Using where
  
  where条件没有索引列，进而通过where的条件再一次过滤获取所需数据。
  
  3. Using temporary
  
  表示查询后的结果需要使用临时表存储。一般在排序或者分组中用到。
  
  4. Using filesort
  
  表示无法利用索引完成排序。也就是order by 的字段没有建立索引。通常这样的sql都是需要优化的。
  
  5. Using join buffer
  
  在联表查询，如果两个表的连接字段没有建立索引。那么需要一个连接缓冲区来存储中间结果。
  
  6. Impossible where
  
  使用了不太正确的where语句，导致没有符合条件的行。
  
  7. No tables used
  
  查询中没有使用到任何表。即没有from子句。
  
  更多信息，详见 https://dev.mysql.com/doc/refman/5.7/en/explain-output.html#jointype_index_merge
  
  
