# 第十二天学习笔记

## 接上文


3. 自增约束:
    ```sql
    CREATE TABLE user3(
        id INT PRIMARY KEY AUTO_INCREMENT,
        name VARCHAR(20)
    );
    ```

    运行DESCRIBE user3;
    | Field | Type        | Null | Key | Default | Extra          |
    |----   |----         |----  |---- |----     |----            |
    | id    | int(11)     | NO   | PRI | NULL    | auto_increment |
    | name  | varchar(20) | YES  |     | NULL    |                |

    ```sql
    INSERT INTO user3(name) VALUES('张三');
    INSERT INTO user3(name) VALUES('李四');
    ```

    | id | name |
    |----|----  |
    |  1 | 张三 |
    |  2 | 李四 |
    没有自定义id值 但是自动生成了id
          
4. 唯一约束:
    ```sql
    CREATE TABLE user5(
        id INT PRIMARY KEY AUTO_INCREMENT,
        name VARCHAR(20)
    );
    ```
    运行 DESCRIBE user5;

    | Field | Type        | Null | Key | Default | Extra          |
    |----   |----         |----  | ----|----     | ----           |
    | id    | int(11)     | NO   | PRI | NULL    | auto_increment |
    | name  | varchar(20) | YES  |     | NULL    |                |

    新增name为唯一约束:
    ```sql
    ALTER TABLE user5 ADD UNIQUE(name);
    ```
    运行 DESCRIBE user5;

    | Field | Type        | Null | Key | Default | Extra          |
    |----   |----         |----  | ----|----     | ----           |
    | id    | int(11)     | NO   | PRI | NULL    | auto_increment |
    | name  | varchar(20) | YES  | UNI | NULL    |                |
    测试:插入数据
    ```sql
    INSERT INTO user5(name) VALUES ('cc');
    ```
    运行
    ```sql
    SELECT * FROM user5; 
    ```
    查看结果:

    | id | name |
    |----|----  |
    |  1 | cc   |
    
    主键约束(primary key)中包含了唯一约束

    场景:业务需求:设计一张用户注册表,用户姓名必须要用手机号来注册,而且手机号和用户名称都不能为空,那么设计出来的结构如下:
    ```sql
    CREATE TABLE user_test(
        id INT PRIMARY KEY AUTO_INCREMENT COMMENT'主键id',
        name VARCHAR(20) NOT NULL COMMENT'用户姓名,不能为空',
        phone_number VARCHAR(20) UNIQUE NOT NULL COMMENT'用户手机,不能重复且不能为空'
    );
    ```
    运行 
    ```sql
    DESCRIBE user_test;
    ```
    | Field        | Type        | Null | Key | Default | Extra          |
    |----          | ----        | ---- |---- |----     |----            |
    | id           | int(11)     | NO   | PRI | NULL    | auto_increment |
    | name         | varchar(20) | NO   |     | NULL    |                |
    | phone_number | int(11)     | NO   | UNI | NULL    |                |
    这样的话就达到了每一个手机号都只能出现一次,达到了每个手机号只能被注册一次。用户姓名可以重复,但是手机号码却不能重复,复合正常的逻辑需求。
5. 非空约束:
    
    见上一条的name约束
6. 默认约束
    ```sql
    CREATE TABLE user6(
    id int PRIMARY KEY AUTO_INCREMENT COMMENT'主键id',
        name VARCHAR(20) NOT NULL COMMENT'用户姓名不能为空',
        phone_number VARCHAR(20) NOT NULL COMMENT'用户手机号,不能为空',
        status INT DEFAULT 0 COMMENT'用户状态0:启用 1:禁封 默认:0'
    );
    ```

    应用场景:
    业务需求:找正常的用户,对这些正常用户进行发放优惠卷或者积分之类的东西,而被禁封的用户我们不让其参加多动.
    我们想要封用户只要将status的值从0改为1就行了,当然我们取用户的时候必须要先判断status是否是0.若是1.说明该用户已经被禁封.
7. 外键约束
    ```sql
    CREATE TABLE classes(
        id INT PRIMARY KEY AUTO_INCREMENT COMMENT'班级表id',
        name VARCHAR(20) COMMENT'班级名称'
    );
    ```
    运行DESCRIBE classes;

    | Field | Type        | Null | Key | Default | Extra          |
    |----          | ----        | ---- |---- |----     |----            |
    | id    | int(11)     | NO   | PRI | NULL    | auto_increment |
    | name  | varchar(20) | YES  |     | NULL    |                |

    ```sql
    CREATE TABLE student(
    id INT PRIMARY KEY AUTO_INCREMENT COMMENT'学生表id',
    name VARCHAR(20) COMMENT'学生姓名',
        class_id int COMMENT'教室id,这张表中的class_id是classes表中id的值',
        FOREIGN KEY (class_id) REFERENCES classes(id)
    );
    //FOREIGN :外来  REFERENCES:应用,参考
    ```
    运行DESCRIBE student;

    | Field    | Type        | Null | Key | Default | Extra          |
    |----          | ----        | ---- |---- |----     |----            |
    | id       | int(11)     | NO   | PRI | NULL    | auto_increment |
    | name     | varchar(20) | YES  |     | NULL    |                |
    | class_id | int(11)     | YES  | MUL | NULL    |                |


    班级插入数据:
    ```sql
    INSERT INTO CLASSES (name) VALUES ('一班');
    INSERT INTO CLASSES (name) VALUES ('二班');
    INSERT INTO CLASSES (name) VALUES ('三班');
    INSERT INTO CLASSES (name) VALUES ('四班');
    ```
    查看数据 

    | id | name |
    |----|----  |
    |  1 | 一班 |
    |  2 | 二班 |
    |  3 | 三班 |
    |  4 | 四班 |
    学生插入数据:
    ```sql
    INSERT INTO student (name,class_id) VALUES ('小赵',1);
    INSERT INTO student (name,class_id) VALUES ('小钱',2);
    INSERT INTO student (name,class_id) VALUES ('小孙',3);
    INSERT INTO student (name,class_id) VALUES ('小李',4);
    ```
    查看数据

    | id | name | class_id |
    |----|----  |----      |
    |  1 | 小赵 |        1 |
    |  2 | 小钱 |        2 |
    |  3 | 小孙 |        3 |
    |  4 | 小李 |        4 |
    
- 总结:
    1. 主表中没有的数据,在附表中,是不可以使用的.

    2. 主表中记录的数据现在正在被附表所引用,那么主表中正在被引用的数据不可以被删除

    3. 若要想删除,先将附表中的数据删除在删除主表数据

    4. 对于外键约束大家可以联想 省,市 来进行联想 (市必须要依赖于省,只要省还有一个市在引用,那么就不可以删除省,要不然市就没有省了. 那么我们想删除省,必须要将该省下所有的市全部删除之后,才可以删除这个省)

8. 如何建表之后添加主键约束
    ```sql
    CREATE TABLE user4(
        id INT,
        name VARCHAR(20)
    );
    ```
    运行DESCRIBE user4;

    | Field | Type        | Null | Key | Default | Extra |
    |----          | ----        | ---- |---- |----     |----            |
    | id    | int(11)     | YES  |     | NULL    |       |
    | name  | varchar(20) | YES  |     | NULL    |       |

    加入主键约束:
    ```sql
    ALTER TABLE user4 add PRIMARY KEY(id);
    ```
    再次运行DESCRIBE user4;

    | Field | Type        | Null | Key | Default | Extra |
    |----          | ----        | ---- |---- |----     |----            |
    | id    | int(11)     | NO   | PRI | NULL    |       |
    | name  | varchar(20) | YES  |     | NULL    |       |

    删除主键约束:
    ```sql
    ALERT TABLE user4 DROP PRIMARY KEY;
    ```
    运行DESCRIBE user4查看表结构:

    | Field | Type        | Null | Key | Default | Extra |
    |----          | ----        | ---- |---- |----     |----            |
    | id    | int(11)     | NO   |     | NULL    |       |
    | name  | varchar(20) | YES  |     | NULL    |       |

    使用modify 修改字段.添加约束:
    ```sql
    ALTER TABLE user4 MODIFY id INT PRIMARY key;
    ```
    使用DESCRIBE user4 查看表结构:

    | Field | Type        | Null | Key | Default | Extra |
    |----          | ----        | ---- |---- |----     |----            |
    | id    | int(11)     | NO   | PRI | NULL    |       |
    | name  | varchar(20) | YES  |     | NULL    |       |

    给主键设置自增长:
    ```sql
    ALTER TABLE user4 MODIFY id INT AUTO_INCREMENT;
    ```
    运行 DESCRIBE user4 查看表结构:

    | Field | Type        | Null | Key | Default | Extra          |
    |----          | ----        | ---- |---- |----     |----            |
    | id    | int(11)     | NO   | PRI | NULL    | auto_increment |
    | name  | varchar(20) | YES  |     | NULL    |                |


