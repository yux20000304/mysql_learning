# 第十三天学习笔记

## 数据库的三大设计范式

1. 第一范式（1NF）

    数据表中所有的字段都是不可分割的原子
    ```sql
    create table student(
        id int primary key,
        name varchar(20),
        address varchar(30)
    );
    ```
    插入一段信息
    ```sql
    insert into student values(1,'yyx','wuhan');
    ```
    如果字段值还是可以继续拆分的就不满足第一范式，比如如果这里的地址可以继续拆分成省市街道等，就不满足第一范式。一般拆分的越详细越好。
2. 第二范式（2NF）

    在满足第一范式的情况下，第二范式要求，除了主键外的每一列都必须完全依赖主键。（主要是有的主键是由多个列组合而成的）
3. 第三范式（3NF）

    在满足第二范式的前提下，除开主键列的其他列不能够有传递依赖关系。

### mysql练习

1. 查询语句练习

    创建学生表：
    student
    学号，姓名，性别，出生年月日，所在班级
    ```sql
    create table student(
        student_number varchar(20) primary key,
        student_name varchar(20) not null,
        student_sex varchar(20) not null,
        student_birthday datetime not null,
        class varchar(20)
    );
    ```

    课程表：
    course
    课程号，课程名称，教师编号
    ```sql
    create table course(
        class_number varchar(20) primary key,
        class_name varchar(20) not null,
        teacher_number varchar(20) not null,
        foreign key(teacher_number) references teacher(teacher_number)
    );
    
    ```
    成绩表：
    score
    学号，课程号，成绩
    ```sql
    create table score(
        student_number varchar(20) primary key,
        class_number varchar(20) not null,
        score decimal,
        foreign key(student_number) references student(student_number),
        foreign key(class_number) references course(class_number)
    );
    ```
    教师表：
    teacher
    教师编号，教师姓名，教师性别，出生年月日，职称，所在部门
    ```sql
    create table teacher(
        teacher_number varchar(20) primary key,
        teacher_name varchar(20) not null,
        teacher_sex varchar(20) not null,
        teacher_birthday datetime,
        teacher_level varchar(20) not null,
        department varchar(20) not null
    );
    ```

    往数据表中添加数据

    添加学生数据
    ```sql
    insert into student values('108','曾华','男','1997-09-01','95033');
    insert into student values('105','小明','男','1998-03-01','95031');
    insert into student values('107','小红','女','1999-02-12','95033');
    insert into student values('106','李平','男','1997-12-01','95031');
    insert into student values('104','张鹏','男','1998-07-03','95033');
    insert into student values('109','张三','男','1997-11-03','95031');
    ```
    添加教师数据


    