### SQL

1. 数据库设计三大范式
   ```
   第一范式：数据库表的每一个字段都是不可分割的。
   第二范式：数据库表中的非主属性只依赖于主键。
   第三范式：不存在非主属性对关键字的传递函数依赖关系。
   
   
   第一范式: 属性不可再分.
   第二范式: 在一范式的基础上, 要求数据库表中的每个实例或行必须可以被惟一地区分. 通常需要为表加上一个列, 以存储各个实例的惟一标识. 这个惟一属性列被称为主关键字或主键.
   第三范式: 在二范式的基础上, 要求一个数据库表中不包含已在其它表中已包含的非主关键字信息. 所以第三范式具有如下特征：1). 每一列只有一个值. 2). 每一行都能区分. 3). 每一个表都不包含其他表已经包含的非主关键字信息.
   
   第一范式：1NF 是对属性的原子性约束，要求属性具有原子性，不可再分解；
   第二范式：2NF 是对记录的惟一性约束，要求记录有惟一标识，即实体的惟一性；
   第三范式：3NF 是对字段冗余性的约束，即任何字段不能由其他字段派生出来，它要求字段没有冗余。。
   
   范式化设计优缺点:
   可以尽量得减少数据冗余，使得更新快，体积小
   缺点:对于查询需要多个表进行关联，减少写得效率增加读得效率，更难进行索引优化
   反范式化:
   优点:可以减少表得关联，可以更好得进行索引优化
   ```
   
2. 数据库事务、隔离级别
   - ```
      A（Atomic）：原子性，构成事务的所有操作，要么都执行完成，要么全部不执行，不可能出现部分成功部分失败的情况。
     ​ C（Consistency）：一致性，在事务执行前后，数据库的一致性约束没有被破坏。比如：张三向李四转100元，转账前和转账后的数据是正确状态这叫一致性，如果出现张三转出100元，李四账户没有增加100元这就出现了数据错误，就没有达到一致性。
     ​ I（Isolation）：隔离性，数据库中的事务一般都是并发的，隔离性是指并发的两个事务的执行互不干扰，一个事务不能看到其他事务运行过程的中间状态。通过配置事务隔离级别可以避脏读、重复读等问题。
     ​ D（Durability）：持久性，事务完成之后，该事务对数据的更改会被持久化到数据库，且不会被回滚。
     
     ```
   ```
      
   - 数据库事务的隔离级别有4种，由低到高分别为Read uncommitted 、Read committed 、Repeatable read 、Serializable 。而且，在事务的并发操作中可能会出现脏读，不可重复读，幻读。
   
   ```
      read uncommitted（读未提交）: 一个事务还没提交时，它做的变更就能被别的事务看到，读取尚未提交的数据，哪个问题都不能解决；
   
      read committed（读已提交）：一个事务提交之后，它做的变更才会被其他事务看到，读取已经提交的数据，可以解决脏读 ---- oracle默认的；
   
      repeatable read（可重复读）：一个事务执行过程中看到的数据，总是跟这个事务在启动时看到的数据是一致的，可以解决脏读和不可重复读 ---mysql默认的；
   
      serializable（串行化）：顾名思义是对于同一行记录，“写”会加“写锁”，“读”会加“读锁”。当出现读写锁冲突的时候，后访问的事务必须等前一个事务执行完成，才能继续执行。可以解决脏读、不可重复读和虚读---相当于锁表。
   
      ```
   
      ```
   
3. 什么是索引，有什么优点？

   ```
   索引象书的目录类似，索引使数据库程序无需扫描整个表，就可以在其中找到所需要的数据，索引包含了一个表中包含值的列表，其中包含了各个值的行所存储的位置，索引可以是单个或一组列，索引提供的表中数据的逻辑位置，合理划分索引能够大大提高数据库性能。
   
   哈希索引：
   只有 memory（内存）存储引擎支持哈希索引，哈希索引用索引列的值计算该值的 hashCode，然后在 hashCode 相应的位置存执该值所在行数据的物理位置，因为使用散列算法，因此访问速度非常快，但是一个值只能对应一个 hashCode，而且是散列的分布方式，因此哈希索引不支持范围查找和排序的功能
   
   B+Tree 索引
   正常情况下，如果不指定索引的类型，那么一般是指 B+Tree 索引（或者 B+Tree 索引）。
   存储引擎以不同的方式使用 B+Tree 索引。性能也各有不同，但是 InnoDB 按照原数据格式进行存储。
   B+Tree 索引能够加快数据的读取速度，因为存储引擎不再需要进行全表扫描来获取需要的数据，相反是从索引的根节点开始进行搜索，通过相应的指针移动，最终存储引擎要么找到了对应的值，要么该记录不存在。树的深度与表的大小直接相关。
   B+Tree 索引是按照顺序组织存储的，所以适合范围查找数据
   B+Tree 索引使用与全键值、键值范围或者键前缀查找，其中键前缀进适用于根据最左前缀的查找。
   
   https://xie.infoq.cn/article/f0231cd29e814cc04ba4364db
   缓存一直
   https://xie.infoq.cn/article/0053b76bedbd7dd40137beecb
   
   ```

4. 聚集索引和非聚集索引区别

   ```
   聚集索引:将数据和索引放到一块，并按照一定顺序组织，找到索引即找到数据，数据物理存放的数据和索引顺序是一致的
   非聚集索引:叶子节点不存储数据、存储的是数据行地址
   
   优势
   通过聚集索引可以直接获取数据，相比非聚集索引需要第二次查询
   聚集索引对于范围查询很快、因为数据按照大小排列的
   聚集索引适合在排序场合、非聚集索引不合适
   劣势
   维护索引很昂贵
   表因为使用UUID作为主键，使数据存储稀疏，
   ```

5. 索引设计原则

   - 适合索引的列是出现在Where子句中的列，或者连接子句中指定的列
   - 基数较小的类、索引效果差、没有必要在此列建立索引
   - 不要过度索引、索引需要额外的磁盘空间、并降低读写性能
   - 更新频繁的不要建立索引
   - 尽量扩展索引(联合索引)。不要新建索引
   - 对于查询中很少涉及的列，重复值比较多的列不要建立索引
   - 对于定于text、image、bit类型的列不要建立索引

6. 慢查询explain

   慢的原因是什么？查询条件没有命中索引？是load不了需要的数据列？数据量太大？

   ```
   首先分析语句是不是load额外的数据,可能是查询多余的行且抛弃掉了，可能是加载了许多结果中不需要的列，对SQL语句进行分析并重写
   分析语句的执行计划是不是没有命中索引、使得语句尽可能命中索引
   如果语句已经无法优化、是否数据量太大、如果是不是考虑纵向或横向切分
   ```

   ```mysql
   type
   const:通过索引一次命中，匹配一行数据
   system:表中只有一行记录，相当于系统表
   eq_ref:唯一索引扫描，对于每个索引键，表中只有一条记录与之匹配
   ref:非唯一索引扫描，返回匹配某个值的所有
   range:只检索给定范围的行，使用一个索引来选择行，一般用于between 、<、>
   index:只历遍索引数
   ALL:全表扫描，查询性能最差，随表数据量越多，查询越慢
   
   执行效率
   ALL < index <range <ref <eq_ref <const<system
   ```

7. SQL查询语句优化

   - 表变量替代临时表、避免大事务
   - 避免全表扫描、条件慎用
   - 实践中如何优化MySQL 一下顺序
     - SQL语句及索引的优化

     - 数据库表结构的优化

     - 系统配的优化

     - 硬件的优化

8. 锁的优化策略
   - 读写分离
   
   - 分段加锁
   
     ```
     举例来说 A 事务持有 X1 锁 ，申请 X2 锁，B 事务持有 X2 锁，申请 X1 锁。A 和 B 事务持有锁并且申请对方持有的锁进入循环等待，就造成了死锁
     
     
     阿里二面：怎么解决MySQL死锁问题的？
     原文链接： https://xie.infoq.cn/article/41285fabb8c4ca612d150b415
     ```
   
     
   
- 减少锁的持有时间
  
   - 多个线程尽量相同以相同的顺序去获取资源
   
     ```
     如何尽可能避免死锁
     合理的设计索引，区分度高的列放到组合索引前面，使业务 SQL 尽可能通过索引定位更少的行，减少锁竞争。
     
     调整业务逻辑 SQL 执行顺序， 避免 update/delete 长时间持有锁的 SQL 在事务前面。
     
     避免大事务，尽量将大事务拆成多个小事务来处理，小事务发生锁冲突的几率也更小。
     
     以固定的顺序访问表和行。比如两个更新数据的事务，事务 A 更新数据的顺序为 1，2;事务 B 更新数据的顺序为 2，1。这样更可能会造成死锁。
     
     在并发比较高的系统中，不要显式加锁，特别是是在事务里显式加锁。如 select … for update 语句，如果是在事务里（运行了 start transaction 或设置了autocommit 等于0）,那么就会锁定所查找到的记录。
     
     尽量按主键/索引去查找记录，范围查找增加了锁冲突的可能性，也不要利用数据库做一些额外额度计算工作。比如有的程序会用到 “select … where … order by rand();”这样的语句，由于类似这样的语句用不到索引，因此将导致整个表的数据都被锁住。
     
     优化 SQL 和表设计，减少同时占用太多资源的情况。比如说，减少连接的表，将复杂 SQL 分解为多个简单的 SQL。
     ```
   
     
   
9. 大数据量如何优化、切分?

10. MySQL主从

11. B-Tree B+Tree

    ```
    B-Tree
    叶节点具有相同的深度，叶节点的指针为空
    所有索引不重复
    节点中的数据索引从左到右递增排列
    B+Tree
    非叶子节点不存储data、只存储索引（冗余）、可以放更多的索引
    叶子节点包含所有的索引字段
    叶子节点用指针连接，提高区间访问的性能
    ```




MYSQL

1. 请你说说 mysql 端口
2. MySQL 常用那些引擎，区别是什么，两者索引底层数据是，为什么用他，哪个查询快一点
3. 请你说说事务隔离级别 Mysql 默认级别 其他数据库默认级别
4. 请你说说 Mysql 索引
5. MySQL 两个引擎，myism，innodb 区别，索引，B+树，sql 如何优化
6. 请你说说事务隔离级别 Mysql 默认级别 其他数据库默认级别
7. MySQL 中 order by 语句可以对多个列进行排序，并且使用索引进行加速，是正确的吗？
8. 为什么建议 mysql 的主键是自增的
9. mysql 的网站注入，5.0 以上和 5.0 以下有什么区别？
10. 请简要描述 MySQL 数据库联合索引的命中规则，可举例说明。
11. MySQL 索引一般使用什么数据结构？
12. 请问如果 mysql 中用户密码丢了怎么办，建一个数据库表，授权命令是什么
13. MySQL 的分支版本是什么？
14. Mysql 中，哪种删除 sql 命令是错误的？
15. mysql 的四种隔离状态
16. 请你介绍一下 mysql 的主从复制？
17. .B+树索引怎么实现的
18. 说一下 Mysql 索引有哪些
19. 主从读写数据库中，如果消息交换延迟、拥塞怎么解决？Redis？（说了增加带宽、优先级队列） MySQL 面试题：主从复制 binlog 延迟太多怎么办
20. innodb rr 级别使用 gap lock 是不是已经解决了幻读问题 为什么还要串行化呢？有什么场景没覆盖到吗？
21. SQL 的范式？
22. MySQL 中的锁机制?
23. 乐观锁 ABA 问题的解决？
24. 怎么查看 MySQL 语句有没有用到索引？
25. 聚簇索引与非聚簇索引
26. redo log 和 binlog 的区别
