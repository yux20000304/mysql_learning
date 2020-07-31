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