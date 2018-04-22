#### Mysql架构
- [Mysql逻辑架构（三层）](http://www.cnblogs.com/baochuan/archive/2012/03/15/2397536.html)
- 并发控制
  - 读锁（共享锁）
  - 写锁（排他锁）
  - 表锁 写锁插入队列优先级高于读锁，
  - 行级锁 在存储引擎层（第三层）实现，不是在服务器层（第二层）实现
- 事务（处理占有CPU，内存，磁盘，应该根据是否需要事务处理选择合适的引擎）
  - 特性（ACID)
    - 原子性
    - 一致性
    - 隔离性
    - 持久性
  - 隔离级别
    - `READ UNCOMMITTED`（未提交读） 脏读现象
    - `READ COMMITTED`（提交读） 不可重复读现象
    - `REPEATABLE READ`（可重复读） 幻读现象
    - `SERIALIZABLE`（可串行化）代价高
  - 死锁
    - 死锁检测和超时机制
    - Innodb目前处理死锁的方法就是将持有最少行级排他锁的事务进行回滚。
    - 行为和顺序和存储引擎相关
  - 事务日志
    - 修改数据时只需修改内存的拷贝，把修改行为记录到日志，后台将修改写回磁盘
    - 两次磁盘写操作：日志写入磁盘，根据日志修改数据库写入磁盘
  - Mysql事务存储引擎
    - `InnoDB`
    - `NDB Cluster`
    - 默认`AUTOCOMMIT`，每次操作都被当做一个事务提交
    - 一些命令会强行执行`COMMIT`操作，如`ALTER TABLE`
    - 混合使用引擎
    - 两阶段锁协议
    - 隐式和显式锁定
- [多版本并发控制（MVCC)](https://www.cnblogs.com/chenpingzhao/p/5065316.html)
  - 只在`READ COMMITTED` ，`REPEATABLE READ`下工作
- Mysql的存储引擎
  - 创建表时会创建与表同名的`.frm` 文件保存表的定义，通过`SHOW TABLE STATUS`命令查看
  - InnoDB
    - 数据存储在表空间 
    - 采用MVCC支持高并发,默认隔离级别`REPEATABLE READ`
    - 间隙锁防止幻读
    - 聚簇索引
    - 可预测性预读
    - 自适应哈希索引
    - 插入缓冲器
  - MyISAM(MySQL5.1之前版本的默认引擎）
    - 支持全文索引，压缩，空间函数
    - 不支持事务，行级锁，外键
    - 表存储在两个文件中：数据文件(.MYD)，索引文件(.MYI)
    - 压缩表
  - 其他引擎
  - 尽量选择InnoDB，原因：支持事务，在线热备份，崩溃后恢复性好等等
  - 转换表的引擎 （会丢失原引擎的所有特性）
    - `ALERT TABLE` 执行时间长，涉及表的复制，消耗系统的IO能力，在原表上加读锁
    - 导出与导入 `mysqldump`导出文件，在`CREATE TABLE`语句上修改引擎
    - 创建与查询
    ```
    CREATE TABLE innodb_table LIKE mysiam_table;
    ALTER TABLE innodb_table ENGINE=InnoDB;
    INSERT INTO innodb_table SELECT * FROM mysiam_table;
    ```
		
