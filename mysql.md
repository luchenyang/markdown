### Mysql的事物隔离级别

大家都知道数据库事务具备ACID特性，即Atomicity(原子性) Consistency(一致性), Isolation(隔离性), Durability(持久性)

- 原子性：要执行的事务是一个独立的操作单元，要么全部执行，要么全部不执行

- 一致性：事务的一致性是指事务的执行不能破坏数据库的一致性，一致性也称为完整性。一个事务在执行后，数据库必须从一个一致性状态转变为另一个一致性状态。

- 隔离性：多个事务并发执行时，一个事务的执行不应影响其他事务的执行
- 持久性：

| 隔离级别                       | 脏读 | 不可重复读 | 幻读 | 概念                                                         |
| ------------------------------ | ---- | ---------- | ---- | ------------------------------------------------------------ |
| READ UNCOMMITED（读未提交）    | √    | √          | √    | 事务能够看到其他事务没有提交的修改，当另一个事务又回滚了修改后的情况，又被称为脏读dirty read |
| READ COMMITED（RC  读已提交）  | ×    | √          | √    | 事务能够看到其他事务提交后的修改，这时会出现一个事务内两次读取数据可能因为其他事务提交的修改导致不一致的情况，称为不可重复读 |
| REPEATABLE READ（RR 可重复读） | ×    | ×          | √    | 事务在两次读取时读取到的数据的状态是一致的                   |
| SERILIZABLE（串行化）          | ×    | ×          | ×    | 可重复读中可能出现第二次读读到第一次没有读到的数据，也就是被其他事务插入的数据，这种情况称为幻读phantom read, 该级别中不能出现幻读 |



### Mysql的Explain

expain出来的信息有10列，分别是id、select_type、table、type、possible_keys、key、key_len、ref、rows、Extra

- **id**
  -  id相同时，执行顺序由上至下
  -  如果是子查询，id的序号会递增，id值越大优先级越高，越先被执行
  - id如果相同，可以认为是一组，从上往下顺序执行；在所有组中，id值越大，优先级越高，越先执行

- **select_type**      ***\*表示查询中每个select子句的类型\****
  - SIMPLE(简单SELECT,不使用UNION或子查询等)
  - PRIMARY(查询中若包含任何复杂的子部分,最外层的select被标记为PRIMARY)
  - UNION(UNION中的第二个或后面的SELECT语句)
  - DEPENDENT UNION(UNION中的第二个或后面的SELECT语句，取决于外面的查询)
  - UNION RESULT(UNION的结果)
  - SUBQUERY(子查询中的第一个SELECT)
  -  DEPENDENT SUBQUERY(子查询中的第一个SELECT，取决于外面的查询)
  - DERIVED(派生表的SELECT, FROM子句的子查询)
  - UNCACHEABLE SUBQUERY(一个子查询的结果不能被缓存，必须重新评估外链接的第一行)

- **table**   显示这一行的数据是关于哪张表的

- **type**    表示MySQL在表中找到所需行的方式，又称“访问类型”。

  常用的类型有： **ALL, index, range, ref, eq_ref, const, system, NULL（从左到右，性能从差到好）**

  - ALL：Full Table Scan， MySQL将遍历全表以找到匹配的行

  - index: Full Index Scan，index与ALL区别为index类型只遍历索引树

  - range:只检索给定范围的行，使用一个索引来选择行

  - ref: 表示上述表的连接匹配条件，即哪些列或常量被用于查找索引列上的值

  - eq_ref: 类似ref，区别就在使用的索引是唯一索引，对于每个索引键值，表中只有一条记录匹配，简单来说，就是多表连接中使用primary key或者 unique key作为关联条件

  - const、system: 当MySQL对查询某部分进行优化，并转换为一个常量时，使用这些类型访问。如将主键置于where列表中，MySQL就能将该查询转换为一个常量,system是const类型的特例，当查询的表只有一行的情况下，使用system

  - NULL: MySQL在优化过程中分解语句，执行时甚至不用访问表或索引，例如从一个索引列里选取最小值可以通过单独索引查找完成

- **possible_keys**     **指出MySQL能使用哪个索引在表中找到记录，查询涉及到的字段上若存在索引，则该索引将被列出，但不一定被查询使用**

- **Key**    **key列显示MySQL实际决定使用的键（索引）**

  如果没有选择索引，键是NULL。要想强制MySQL使用或忽视possible_keys列中的索引，在查询中使用FORCE INDEX、USE INDEX或者IGNORE INDEX。

- **key_len**      ***\*表示索引中使用的字节数，可通过该列计算查询中使用的索引的长度（key_len显示的值为索引字段的最大可能长度，并非实际使用长度，即key_len是根据表定义计算而得，不是通过表内检索出的）\****       不损失精确性的情况下，长度越短越好 

- **ref**    **表示上述表的连接匹配条件，即哪些列或常量被用于查找索引列上的值**

- **rows**   **表示MySQL根据表统计信息及索引选用情况，估算的找到所需的记录所需要读取的行数**

- **extra**

  **该列包含MySQL解决查询的详细信息,有以下几种情况：**

  - Using where:列数据是从仅仅使用了索引中的信息而没有读取实际的行动的表返回的，这发生在对表的全部的请求列都是同一个索引的部分的时候，表示mysql服务器将在存储引擎检索行后再进行过滤

  - Using temporary：表示MySQL需要使用临时表来存储结果集，常见于排序和分组查询

  - Using filesort：MySQL中无法利用索引完成的排序操作称为“文件排序”

  - Using join buffer：改值强调了在获取连接条件时没有使用索引，并且需要连接缓冲区来存储中间结果。如果出现了这个值，那应该注意，根据查询的具体情况可能需要添加索引来改进能。

  - Impossible where：这个值强调了where语句会导致没有符合条件的行。

  - Select tables optimized away：这个值意味着仅通过使用索引，优化器可能仅从聚合函数结果中返回一行



### Mysql的事物日志

MySQL Innodb中跟数据持久性、一致性有关的日志，有以下几种：

- Bin Log:是mysql服务层产生的日志，常用来进行数据恢复、数据库复制，常见的mysql主从架构，就是采用slave同步master的binlog实现的

- Redo Log:记录了数据操作在物理层面的修改，mysql中使用了大量缓存，修改操作时会直接修改内存，而不是立刻修改磁盘，事务进行中时会不断的产生redo log，在事务提交时进行一次flush操作，保存到磁盘中。当数据库或主机失效重启时，会根据redo log进行数据的恢复，如果redo log中有事务提交，则进行事务提交修改数据。

- Undo Log: 除了记录redo log外，当进行数据修改时还会记录undo log，undo log用于数据的撤回操作，它记录了修改的反向操作，比如，插入对应删除，修改对应修改为原来的数据，通过undo log可以实现事务回滚，并且可以根据undo log回溯到某个特定的版本的数据，实现MVCC

  

### Mysql的当前读和快照读

#### 当前读

它读取的数据库记录，都是当前最新的版本，会对当前读取的数据进行加锁，防止其他事务修改数据。是悲观锁的一种操作。

如下操作都是当前读：

- select lock in share mode (共享锁)

- select for update (排他锁)

- update (排他锁)

- insert (排他锁)

- delete (排他锁)

- 串行化事务隔离级别

#### Mysql的MVCC(快照读)

全称Multi-Version Concurrency Control，即多版本并发控制，主要是为了提高数据库的并发性能

mvcc的实现主要有3个点

- **版本链**  	对于使用`InnoDB`存储引擎的表来说，它的聚簇索引记录中都包含两个必要的隐藏列（`row_id`并不是必要的，我们创建的表中有主键或者非NULL唯一键时都不会包含`row_id`列）

  - `trx_id`：每次对某条聚簇索引记录进行改动时，都会把对应的事务id赋值给`trx_id`隐藏列。 
  - `roll_pointer`：每次对某条聚簇索引记录进行改动时，都会把旧的版本写入到`undo日志`中，然后这个隐藏列就相当于一个指针，可以通过它来找到该记录修改前的信息。

- **ReadView**

  

- **undoLog**



