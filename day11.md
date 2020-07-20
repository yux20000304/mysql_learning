# 第十一天学习笔记

## 基本的java知识掌握了，感觉想做出一个项目还是比较困难，接下来学习一点sql

1. SQL

    SQL语言是一种结构化查询语言。现在的SQL有很多个版本，对于某些版本的扩展需要使用其特定的格式，大部分功能都是通用的SQL语言。

    - DDL：Data Definition Language

    DDL允许用户定义数据，也就是创建表、删除表、修改表结构这些操作。通常，DDL由数据库管理员执行。

    - DML：Data Manipulation Language

    DML为用户提供添加、删除、更新数据的能力，这些是应用程序对数据库的日常操作。

    - DQL：Data Query Language

    DQL允许用户查询数据，这也是通常最频繁的数据库日常操作。
2. 关系模型

    在关系数据库中，是通过主键和外键来维护的。

    - 主键
  
    对于关系表，有个很重要的约束，就是任意两条记录不能重复。不能重复不是指两条记录不完全相同，而是指能够通过某个字段唯一区分出不同的记录，这个字段被称为主键。主键是唯一用来定位的。选取主键的一个基本原则是：不使用任何业务相关的字段作为主键。一般是分配id。

    - 联合主键

    关系数据库实际上还允许通过多个字段唯一标识记录，即两个或更多的字段都设置为主键，这种主键被称为联合主键。

    - 外键
    通过外键，可以把两张表联系起来。
    
    比如在一个表中多添加一列，这列存储的是另外一张表的主键信息，就可以把这两个表联系起来了。

    外键并不是通过列名实现的，而是通过定义外键约束实现的：
    ```SQL
    ALTER TABLE students
    ADD CONSTRAINT fk_class_id
    FOREIGN KEY (class_id)
    REFERENCES classes (id);
    ```
    其中，外键约束的名称fk_class_id可以任意，FOREIGN KEY (class_id)指定了class_id作为外键，REFERENCES classes (id)指定了这个外键将关联到classes表的id列（即classes表的主键）。

    主键不能为空，不能重复。
3. 在cmd中操作SQL

    1. 如何查看有什么数据库?    
    ```sql
        show databases;
    ```
    2. 如何选择数据库?   
    ```sql
        use databasesName;
    ```
    3. 如何查看该数据库中有哪些表?   
    ```sql
        show tables;
    ```
    4. 如何查询表中的数据?   
    ```sql
        select * from tableName;
    ```
    5. 如何退出数据库服务器?
    ```sql
    exit;
    ```
    6. 如何在数据库服务器中创建自己的数据库?   
    ```sql
    create database databaseName;
    ```
    7. 如何创建一个数据表? 创建一个pet表
    ```SQL
    create TABLE pet(
                    name VARCHAR(20),
                    owner VARCHAR(20),
                    specise VARCHAR(20),
                    sex CHAR(1),
                    brith DATAE,
                    death DATE );
    ```
    **注意事项:**

    - var()与varchar()的区别在于var()是定常的,哪怕存储的字符串没有达到"()"中数字的上限,var()依然会占用空格来填充空间.而varchar()则是不定长的,没有达到"()"中的上限则会自动去掉后面的空格;
   
    - 性别不要用:sex 要用:gender  一个是性 一个是性别;

    - 定义最后一个字段的时候不要加",";

    - 上面的"VAR","VARCHAR","DATE"可以用小写.不过最好用大写来表示区分关键字,若不然也许写到后面你自己都不知道这个词是数据库中的关键字还是你自己自定义的一些数据,同时一定要用英文的标点符号也必须半角输入
    8. 如何查看数据表的架构?   
    ```sql
    describe tableName;
    ```
 
    9.如何插入数据?
    ```sql
    INSERT INTO pet VALUES('kk','cc','dog','1','1998-8-2',null);
    ```
    其实还有一种写法:
     ```sql            
    INSERT INTO pet(name,owner) VALUES ('xx','cc');
    ```
    代表我只在name和owner字段上面插入的一条,其他皆为NULL/默认值的数据
              
    10. mysql 常用数据类型
 
    注意:金钱最好用int/bigint(整数,单位用分,拿出来进行*100换成元),千万不要直接用浮点,会有精度损失.

    11.如何删除数据
    
    先插入数据:
   ```sql
    INSERT INTO pet VALUES('kk1','cc1','dog1','1','1998-1-2',null);
    INSERT INTO pet VALUES('kk2','cc2','dog2','2','1998-2-2',null);
    INSERT INTO pet VALUES('kk3','cc3','dog3','1','1998-3-2','1998-12-2');
    INSERT INTO pet VALUES('kk4','cc4','dog4','2','1998-4-2',null);
    ```

    删除语句:
    ```sql
    DELETE FROM tablesName WHRER 条件;
    ``` 
    修改数据:
    ```sql
    UPDATE tableName SET 字段1=值1,字段2=值2 ... WHERE 条件;
    ```
    12. mysql建表中的约束
    - 主键约束:
    它能够唯一确定一张表中的一条记录,增加主键约束之后,就可以使得字段不重复而且不为空
    ```sql   
    create table user(
        id int PRIMARY KEY,
        name VARCHAR(20)
    );
    INSERT INTO user VALUES (1,'张三');
    ```
    - 复合主键:
    ```sql
    CREATE TABLE user2(
        id INT,
        name VARCHAR(20),
        password VARCHAR(20),
        PRIMARY key(id,name)
    );
    ```

    