# 第十五天学习笔记

## 前面一直学习进度太慢，今天决定快点先把概念都跑完，练习随后就做

1. 条件查询

    ```sql
    select * from ... where ...   ...;
    ```
    三种用法
    - where后面加and表示同时满足
    - where后面加or表示或
    - where后面加not表示不含有
    - not优先级最高，and，or
    - <>表示不相等
    - like判断相似，用来筛选部分相似的数据
2. 投影查询

    把上面的*替换成我们想要的列就可以只显示出我们需要的那几列。

3. 排序

    ```sql
    select * from ... order by (某一列);
    ```
    默认是升序排列asc，如果需要降序则在最后加desc。
4. 分页查询

    数据量太大，可以进行分页显示，每次只显示指定条数。
    ```sql
    select * from ... order by ... desc limit a offset b;
    ```
    这里是表示从b号记录开始，每次最多取a条，如果需要看下一页就需要修改b的值。
5. 聚合查询

    ```sql
    select count(*) from ...;
    ```
    通过上面的语句可以得到一共有多少条记录，上面的*可以使用列名替换，当然也可以在后面加where语句限制，除此之外，sql还提供了以下几种方法：
    - sum方法
    - avg方法
    - max方法
    - min方法
    除此之外，sql还提供了分组的关键字group by根据提供的列进行分组。
6. 多表查询

    ```sql
    SELECT s.id sid,s.name,s.gender,s.score,c.id cid,c.name cname FROM students s, classes c WHERE s.gender = 'M' AND c.id = 1;

    ```
7. 连接查询

    想把另外一个表的内容放在这个表中并且对应起来
    ```sql
    SELECT s.id, s.name, s.class_id, c.name class_name, s.gender, s.score
    FROM students s
    INNER JOIN classes c
    ON s.class_id = c.id;

    ```
    这里我们使用的是inner join关键字，INNER JOIN查询的写法是：

    - 先确定主表，仍然使用FROM <表1>的语法；
    - 再确定需要连接的表，使用INNER JOIN <表2>的语法；
    - 然后确定连接条件，使用ON <条件...>，这里的条件是s.class_id = c.id，表示students表的class_id列与classes表的id列相同的行需要连接；
    - 可选：加上WHERE子句、ORDER BY等子句。
    那么inner join和outer join有什么区别呢
    - inner join只返回同时存在于两张表的行数据
    - right outer join返回右表都存在的行
    - left outer join返回左表都存在的行
    - full outer join会把两张表所有的记录全部选择出来，并且把对方不存在的自动填充为null
8. 增

    insert语句
    ```sql
    INSERT INTO <表名> (字段1, 字段2, ...) VALUES (值1, 值2, ...);
    ```
9. 改

    关键词update
    ```sql
    UPDATE <表名> SET 字段1=值1, 字段2=值2, ... WHERE ...;
    ```
10. 删

    使用的关键字：delete
    ```sql
    DELETE FROM <表名> WHERE ...;
    ```
    和上面的类似
11. 插入或替换

    如果我们希望插入一条新记录（INSERT），但如果记录已经存在，就先删除原记录，再插入新记录。此时，可以使用REPLACE语句，这样就不必先查询，再决定是否先删除再插入：
    ```sql
    REPLACE INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99);
    ```
    若id=1的记录不存在，REPLACE语句将插入新记录，否则，当前id=1的记录将被删除，然后再插入新记录。
12. 插入或忽略

    如果我们希望插入一条新记录（INSERT），但如果记录已经存在，就啥事也不干直接忽略，此时，可以使用INSERT IGNORE INTO ...语句：
    ```sql
    INSERT IGNORE INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99);
    ```
    若id=1的记录不存在，INSERT语句将插入新记录，否则，不执行任何操作。
13. 快照
    如果想要对一个表进行快照，即复制一份当前表的数据到一个新表，可以结合CREATE TABLE和SELECT：
    ```sql
    -- 对class_id=1的记录进行快照，并存储为新表students_of_class1:
    CREATE TABLE students_of_class1 SELECT * FROM students WHERE class_id=1;
    ```
    新创建的表结构和SELECT使用的表结构完全一致。
14. 事务

    把多条语句作为一个整体进行操作的功能称作数据库事务。数据库事务可以确保该事务范围内所有的操作全部成功或者全部失败。

    数据库事务具有ACID特性：
    - A：Atomic，原子性，将所有SQL作为原子工作单元执行，要么全部执行，要么全部不执行；
    - C：Consistent，一致性，事务完成后，所有数据的状态都是一致的，即A账户只要减去了100，B账户则必定加上了100；
    - I：Isolation，隔离性，如果有多个事务并发执行，每个事务作出的修改必须与其他事务隔离；
    - D：Duration，持久性，即事务完成后，对数据库数据的修改被持久化存储。
    1. 如何开辟一个事务？
        1. 显式事务
        ```sql
        BEGIN;
        ...这里填充需要执行的指令
        COMMIT;
        ```
        2. 回滚事务

            有时我们希望任务主动失败可以直接把COMMIT更换为ROLLBACK。
    隔离级别：对于两个并发执行的事务，用来处理数据不一致的情况
        1. read uncommitted

            这个级别是隔离级别中最低的一种级别。两个事务在不同的客户端同时执行，一个事务会读到另外一个事务更新后但是并没有提交的数据，如果发生了事务回滚情况，当前读取的数据就是脏数据。
        2. read committed

            这个隔离级别下，一个事务会遇到不可重复读的问题，这个指的是两个事务在不同的客户端执行，多次读取一个数据，在其中一个事务没有结束的时候，另外一个事务修改了这个数据的值，那么在第一个事务中，就会出现多次读取同一个数据但是不相同的情况。
        3. repeatable read

            这个指的是在一个事务中第一次查询某条记录，发现没有，但是在更新的时候会成功，并且被成功读入了。具体例子就是两个事务在不同的客户端执行，开始没有某个数据，后来在某一个事务中插入了数据，在另外一个事务中反映出来的就是忽然不存在的数据出现了，并且被成功读取了。
        4. serializable

            这是最严格的隔离级别，所有的事务依次执行，相当于变成了串行执行，避免了上述的所有可能的错误情况。

圆满了！