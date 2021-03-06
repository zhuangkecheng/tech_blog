# Join语句
#### join
*  join 语句执行过程中，驱动表是走全表扫描，而被驱动表是走树搜索

#### join结论
* 使用 join 语句，性能比强行拆成多个单表执行 SQL 语句的性能要好；
* 如果使用 join 语句的话，需要让小表做驱动表。

#### BNL
* 把表 t1 的数据读入线程内存 join_buffer 中，由于我们这个语句中写的是 select *，因此是把整个表 t1 放入了内存；
扫描表 t2，把表 t2 中的每一行取出来，跟 join_buffer 中的数据做对比，满足 join 条件的，作为结果集的一部分返回。
* ![15ae4f17c46bf71e8349a8f2ef70d573](media/15488183158298/15ae4f17c46bf71e8349a8f2ef70d573.jpg)
* join_buffer_szie设置大一些

#### join语句判断
* 如果可以使用 Index Nested-Loop Join 算法，也就是说可以用上被驱动表上的索引，其实是没问题的；
* 如果使用 Block Nested-Loop Join 算法，扫描行数就会过多。尤其是在大表上的 join 操作，这样可能要扫描被驱动表很多次，会占用大量的系统资源。所以这种 join 尽量不要用

#### 小表
* 在决定哪个表做驱动表的时候，应该是两个表按照各自的条件过滤，过滤完成之后，计算参与 join 的各个字段的总数据量，数据量小的那个表，就是“小表”，应该作为驱动表。
* 