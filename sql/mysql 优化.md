### mysql 优化

----

* 单表优化

  1. 字段

  * 尽量使用tinyint、smallint\ medium_int，非负则加上unsigned
  * varchar长度只分配真正需要的空间
  * 尽量使用timestamp,而非使用datetime
  * 避免使用null。很难查询优化且占用额外索引空间

  2. 查询sql

  * 开启慢查询日志找出较慢的sql不做列运算
  * or 改成 in
  * 不用select *
  * 不用函数和触发器
  * 少用join
  * 避免在where 子句中使用 != 或 > <操作符
  * 列表数据不要拿全表，要用limit分页

  3. 引擎  myisam   innodb

  * myisam: 不支持事务、不支持外键、不支持行锁
  * innodb: 支持事务、支持外键、支持行锁

----

* 读写分离

-----

* 缓存
  * 数据访问层
  * 应用服务层
  * web 层：针对web页面做缓存
  * 浏览器客户端： 用户端的缓存

直写式： 数据写入数据库后，同时更新缓存，维持数据库与缓存的一致性。实现简单，同步好，效率一般

回写式：当有数据写入数据库是，只会更新缓存，然后异步批量将缓存同步至数据库。实现复杂，可能产生数据库与缓存         				

​	     不同步，效率高

----

* 表分区

-----

* 水平拆分

  实际：分片存储，分库内分表和分库两部分，达到分布式效果

  优点：

  * 不存在单库大数据和高并发性能瓶颈
  * 应用端改造较少
  * 提高了系统的稳定性和负载能力

  缺点：

  * 分片事务一致性难以解决
  * 跨节点 join 性能差，逻辑复杂
  * 数据多次扩展难度跟维护量极大

---

* 垂直拆分

---

* 系统调优

  可以使用下面几个工具来做基准测试：



  - **sysbench：**一个模块化，跨平台以及多线程的性能测试工具。

    https://github.com/akopytov/sysbench

  - **iibench-mysql：**基于Java的MySQL / Percona / MariaDB 索引进行插入性能测试工具。

    https://github.com/tmcallaghan/iibench-mysql

  - **tpcc-mysql：**Percona开发的TPC-C测试工具。

    https://github.com/Percona-Lab/tpcc-mysql



  具体的调优参数内容较多，具体可参考官方文档，这里介绍一些比较重要的参数：



  - **back_log：**back_log值可以指出在MySQL暂时停止回答新请求之前的短时间内多少个请求可以被存在堆栈中。也就是说，如果MySQL的连接数据达到max_connections时，新来的请求将会被存在堆栈中，以等待某一连接释放资源，该堆栈的数量即back_log，如果等待连接的数量超过back_log，将不被授予连接资源。可以从默认的50升至500。
  - **wait_timeout：**数据库连接闲置时间，闲置连接会占用内存资源。可以从默认的8小时减到半小时。
  - **max_user_connection：**最大连接数，默认为0无上限，最好设一个合理上限。
  - **thread_concurrency：**并发线程数，设为CPU核数的两倍。
  - **skip_name_resolve：**禁止对外部连接进行DNS解析，消除DNS解析时间，但需要所有远程主机用IP访问。
  - **key_buffer_size：**索引块的缓存大小，增加会提升索引处理速度，对MyISAM表性能影响最大。对于内存4G左右，可设为256M或384M，通过查询show status like 'key_read%'，保证key_reads / key_read_requests在0.1%以下最好。
  - **innodb_buffer_pool_size：**缓存数据块和索引块，对InnoDB表性能影响最大。通过查询show status like 'Innodb_buffer_pool_read%'，保证 (Innodb_buffer_pool_read_requests – Innodb_buffer_pool_reads) / Innodb_buffer_pool_read_requests越高越好。
  - **innodb_additional_mem_pool_size：**InnoDB存储引擎用来存放数据字典信息以及一些内部数据结构的内存空间大小，当数据库对象非常多的时候，适当调整该参数的大小以确保所有数据都能存放在内存中提高访问效率，当过小的时候，MySQL会记录Warning信息到数据库的错误日志中，这时就需要该调整这个参数大小。
  - **innodb_log_buffer_size：**InnoDB存储引擎的事务日志所使用的缓冲区，一般来说不建议超过32MB。 
  - **query_cache_size：**缓存MySQL中的ResultSet，也就是一条SQL语句执行的结果集，所以仅仅只能针对select语句。当某个表的数据有任何任何变化，都会导致所有引用了该表的select语句在Query Cache中的缓存数据失效。所以，当我们数据变化非常频繁的情况下，使用Query Cache可能得不偿失。根据命中率(Qcache_hits/(Qcache_hits+Qcache_inserts)*100))进行调整，一般不建议太大，256MB可能已经差不多了，大型的配置型静态数据可适当调大。可以通过命令show status like 'Qcache_%'查看目前系统Query catch使用大小。
  - **read_buffer_size：**MySQL读入缓冲区大小。对表进行顺序扫描的请求将分配一个读入缓冲区，MySQL会为它分配一段内存缓冲区。如果对表的顺序扫描请求非常频繁，可以通过增加该变量值以及内存缓冲区大小提高其性能。
  - **sort_buffer_size：**MySQL执行排序使用的缓冲大小。如果想要增加ORDER BY的速度，首先看是否可以让MySQL使用索引而不是额外的排序阶段。如果不能，可以尝试增加sort_buffer_size变量的大小。
  - **read_rnd_buffer_size：**MySQL的随机读缓冲区大小。当按任意顺序读取行时(例如按照排序顺序)，将分配一个随机读缓存区。进行排序查询时，MySQL会首先扫描一遍该缓冲，以避免磁盘搜索，提高查询速度，如果需要排序大量数据，可适当调高该值。但MySQL会为每个客户连接发放该缓冲空间，所以应尽量适当设置该值，以避免内存开销过大。
  - **record_buffer：**每个进行一个顺序扫描的线程为其扫描的每张表分配这个大小的一个缓冲区。如果你做很多顺序扫描，可能想要增加该值。
  - **thread_cache_size：**保存当前没有与连接关联但是准备为后面新的连接服务的线程，可以快速响应连接的线程请求而无需创建新的。
  - **table_cache：**类似于thread_cache _size，但用来缓存表文件，对InnoDB效果不大，主要用于MyISAM。

----



----

* 表分区

  MySQL在5.1版引入的分区是一种简单的水平拆分，用户需要在建表的时候加上分区参数，对应用是透明的无需修改代码。



  对用户来说，分区表是一个独立的逻辑表，但是底层由多个物理子表组成，实现分区的代码实际上是通过对一组底层表的对象封装，但对SQL层来说是一个完全封装底层的黑盒子。MySQL实现分区的方式也意味着索引也是按照分区的子表定义，没有全局索引。



  ![img](https://mmbiz.qpic.cn/mmbiz_png/tibrg3AoIJTuZEnaHbicCWqdBBXsmktd6f070G5csssyRib1UuRp4cGywGsOA6ib3JOGeBlPfrPjSlfmPFMK46rPxg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



  用户的SQL语句是需要针对分区表做优化，SQL条件中要带上分区条件的列，从而使查询定位到少量的分区上，否则就会扫描全部分区，可以通过EXPLAIN PARTITIONS来查看某条SQL语句会落在那些分区上，从而进行SQL优化，如下图5条记录落在两个分区上：





  mysql> explain partitions select count(1) from user_partition where id in (1,2,3,4,5);

  +----+-------------+----------------+------------+-------+---------------+---------+---------+------+------+--------------------------+

  | id | select_type | table          | partitions | type  | possible_keys | key     | key_len | ref  | rows | Extra                    |

  +----+-------------+----------------+------------+-------+---------------+---------+---------+------+------+--------------------------+

  |  1 | SIMPLE      | user_partition | p1,p4      | range | PRIMARY       | PRIMARY | 8       | NULL |    5 | Using where; Using index |

  +----+-------------+----------------+------------+-------+---------------+---------+---------+------+------+--------------------------+

  1 row in set (0.00 sec)



  **4.1 分区的好处是：**



  - 可以让单表存储更多的数据；
  - 分区表的数据更容易维护，可以通过清楚整个分区批量删除大量数据，也可以增加新的分区来支持新插入的数据，另外，还可以对一个独立分区进行优化、检查、修复等操作；
  - 部分查询能够从查询条件确定只落在少数分区上，速度会很快；
  - 分区表的数据还可以分布在不同的物理设备上，从而搞笑利用多个硬件设备；
  - 可以使用分区表赖避免某些特殊瓶颈，例如InnoDB单个索引的互斥访问、ext3文件系统的inode锁竞争；
  - 可以备份和恢复单个分区。



  **4.2 分区的限制和缺点：**



  - 一个表最多只能有1024个分区；
  - 如果分区字段中有主键或者唯一索引的列，那么所有主键列和唯一索引列都必须包含进来；
  - 分区表无法使用外键约束；
  - NULL值会使分区过滤无效；
  - 所有分区必须使用相同的存储引擎。



  **4.3 分区的类型：**



  - **RANGE分区：**基于属于一个给定连续区间的列值，把多行分配给分区。
  - **LIST分区：**类似于按RANGE分区，区别在于LIST分区是基于列值匹配一个离散值集合中的某个值来进行选择。
  - **HASH分区：**基于用户定义的表达式的返回值来进行选择的分区，该表达式使用将要插入到表中的这些行的列值进行计算。这个函数可以包含MySQL中有效的、产生非负整数值的任何表达式。
  - **KEY分区：**类似于按HASH分区，区别在于KEY分区只支持计算一列或多列，且MySQL服务器提供其自身的哈希函数。必须有一列或多列包含整数值