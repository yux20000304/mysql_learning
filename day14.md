# 第十四天学习笔记

## 查询表的练习

1. 查询student表中的所有的记录
    ```sql
    select * from student;
    ```
2. 查询student表中所有记录的student_name,student_sex和class列
    ```sql
    select student_name from student;
    select student_sex from student;
    select class from student;
    ```
3. 查询教师所有的单位即不重复的depart列
    ```sql
    select distinct depart from teacher;
    ```
4. 查询score表中成绩在60~80之间的
    
    使用between...and语句来查询区间
    ```sql
    select * from score where score between 60 and 86;
    ```
    或者
    ```sql
    select * from score where score > 60 and score < 86;
    ```
5. 查询score表中成绩为85，86或88的记录

    使用in关键字
    ```sql
    select * from score where score in (85,86,88);
    ```
6. 查询student表中“95031”班或性别为女的同学记录
    ```sql
    select * from student where class='95031' or student_sex='女';
    ```
7. 以降序查询student表中所有的记录

    降序使用order by ... desc关键词
    ```sql
    select * from student order by class desc;
    ```
    升序使用order by ... asc关键词,这里的asc是默认的，一般可以不写
    ```sql
    select * from student order by class asc;
    ```
8. 以class_number升序，score降序查询score表中的所有记录

    注意到这里谁在前，就先以在前的那个进行排序，再依次对第一项相同的项的第二项进行降序。
    ```sql
    select * from score order by class_number asc, score desc;
    ```
9. 查询“95031”班的学生人数

    统计关键字 count
    ```sql
    select count(*) from student where class='95031';
    ```
10. 查询score表中的最高分和学号

    子查询或者排序
    ```sql
    select student_number,class_number from score where score=(select max(score) from score);
    ```
    这里使用的是子函数嵌套的形式，先定位最高分，再找最高分的信息。
    
    排序的做法：
    ```sql
    select student_number,class_number from score order by score desc limit 0,1;
    ```
    这里的后面加的limit代表从第0条开始到第1条，所以只显示出第一个最高分。
11. 查询每门课的平均成绩

    ```sql
    select avg(score) from score where class_number='3-105';
    ```
    d但是我们这里只能一门一门查，如何一句话计算所有的？
    ```sql
    select class_number,avg(score) from score group by class_number;
    ```
    注意到这里我们使用的是一个group by关键字，用来分组，先按照课程号分组，就可以把所有分组都输出出来。
12. 查询score表中至少有两名学生选修的并且以3开头的课程的平均分数

    ```sql
    select class_number,avg(score) from score group by class_number having count(class_number)>=2 and class_number like '3%';
    ```
    注意到这里的having和where类似，区别是having是在分组之后的，where是在分组之前的。
13. 查询分数大于70，小于90的student_number列
    
    ```sql
    select student_number,score from score where score>70 and score<90;
    ```
    同样的这里可以使用where语句
14. 查询所有学生的student_name,class_number和score列
    
    这三个来自于不同的表,
    ```sql
    select student_number,student_name from student;
    select student_number,class_number,score from score;
    ```