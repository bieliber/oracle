# 实验5：学校信息管理系统
## 罗莹 软工4班 201810414405
### 一、概述
目前在管理.上越来越多并且广泛的应用信息技术，管理信息系统在实施过程中技术日渐成熟。管理信息系统被称为发展着的新型学科，无论哪个单位要发展要生存，要把内部活动高效有序地组织好，就要有一个与自身情况相符合的管理信息系统。
学校在教务管理方面有一个重要的事关学生情况的系统被称为学生的信息管理系统，当前我国的绝大多数学校档案管理的水平依然停留在纸质档案上，这种水平已经十分落后，跟不上时代的发展，因为它既浪费物力又浪费人力，在当今这个信息时代，利用计算机工作的信息管理必然将传统的那种管理方法淘汰。利用学生管理这个系统，能够实现科学统计，快速查询以及信息的规范管理，使管理上的工作量减。毫无疑问，将计算机管理有效地运用到学校的教务管理工作中，能够改善学校的管理制度，并且提升办学水平和教学质量。
### 二、系统需求分析
#### 1.系统功能需求
学生信息管理系统主要是要对学生进行管理,他管理着学生- -些基本的信息，包括学生的个人信息、班级信息以及课程信息等。同时学生信息管理系统也是学校的一个重要组成部分，它有利于学生档案、变动情况以及一些统计信息的有效管理。
学生信息管理系统是根据学校有关管理的- -些客观要求建立起来的，它具有以下几个功能:
1.要将学生的基本信息与资料可以在系统中自行的输入、修改、查询、删除。
2.可以使学校更加方便的查询信息，有利于管理人员对学生情况的了解。
3.还必须可以对数据库的信息进行登记和清理。
#### 2.系统数据需求
- 数据在录入和处理的过程中要具有准确性
数据在输入的过程中必须要对数据进行准确的处理，保证数据的准确性，如果在系统中输入- -些错误信息或数据则会使系统的工作处在一个无意义的工作之中。当前对数据的输入主要还以手工输入为主，手工输入容易出现- -些错误因此要通过系统界面的安排来降低出错率。
- 数据要具有一致性和完整性
学生信息管理系统要按照高标准来对数据进行处理，对学生信息处理方面要尽可能的保障数据的一致性，对录入数据的去向进行控制，并且还要保障数据的完整性。
在数据输入的过程中必须要按照完整性的规则来对系统进行要求，如果不符合数据的完整性，系统可以对其进行拒绝。
- 数据的独立性
学生信息管理系统对学生的信息还必须要进行相应的保护，因此不可能使每一个人对内部信息进行操作，所以只有特定的管理人员来对学生信息管理系统中的信息进行龙智管理，采取独立操作，因此数据具有了独立性。
### 三、数据库设计
#### 1.创建表空间和用户
- 创建表空间
SQL> create tablespace hostspace
datafile 'F:\A学习文件夹\课内学习\大三（下）\Oracle 12C数据库基础教程\\oracle\\test6\\hostspace.dbf'
size 50m
autoextend on
next 5m
maxsize 100m;
- 创建用户
create user study identified by study default tablespace hostspace;
CREATE USER ll IDENTIFIED BY ll PROFILE DEFAULT DEFAULT TABLESPACE LL ACCOUNT UNLOCK;
create user ly identified by ly default tablespace ly_data temporary tablespace ly_temp;
grant connect, resource TO ll;
grant create session to ll;
grant connect,resource to study; 
grant dba to study;

- 创建students角色，和student用户
CREATE ROLE students;
GRANT connect,resource,CREATE VIEW TO students;
CREATE USER user_lft IDENTIFIED BY 123 DEFAULT TABLESPACE users TEMPORARY TABLESPACE temp;
ALTER USER user_lft QUOTA 50M ON users;
GRANT student TO user_lft;
exit

- 创建teachers角色，和teacher用户
CREATE ROLE teachers;
CREATE USER teacher IDENTIFIED BY 123 DEFAULT TABLESPACE users TEMPORARY TABLESPACE temp;
GRANT teacher TO teacher;

#### 2.创建5个表，添加索引
- 学生信息表
CREATE TABLE students(
	"ID" NUMBER, 
	"NAME" VARCHAR2(50 BYTE), 
	"CLASS_ID" NUMBER, 
	"AGE" NUMBER(*,0), 
	"SEX" VARCHAR2(50 BYTE)
) SEGMENT CREATION DEFERRED 
PCTFREE 10 PCTUSED 40 INITRANS 1 MAXTRANS 255 
NOCOMPRESS LOGGING
TABLESPACE "hostspace" ;

- 课程信息表
CREATE TABLE course(	
    "ID" NUMBER, 
	"NAME" VARCHAR2(50 BYTE), 
	"CREDIT" NUMBER(*,0)
) SEGMENT CREATION DEFERRED 
PCTFREE 10 PCTUSED 40 INITRANS 1 MAXTRANS 255 
NOCOMPRESS LOGGING
TABLESPACE "hostspace" ;

- 课表信息表
CREATE TABLE timetable(	
    "ID" NUMBER, 
	"timetable_ID" NUMBER, 
	"CLASS_ID" NUMBER, 
	"TEACHER_ID" NUMBER, 
	"TIME" DATE
) SEGMENT CREATION DEFERRED 
PCTFREE 10 PCTUSED 40 INITRANS 1 MAXTRANS 255 
NOCOMPRESS LOGGING
TABLESPACE "hostspace" ;

- 班级信息表
CREATE TABLE class(	
    "ID" NUMBER, 
	"NAME" VARCHAR2(50 BYTE)
) SEGMENT CREATION DEFERRED 
PCTFREE 10 PCTUSED 40 INITRANS 1 MAXTRANS 255 
NOCOMPRESS LOGGING
TABLESPACE "hostspace" ;

- 教师信息表
CREATE TABLE teachers (	
    "ID" NUMBER, 
	"NAME" VARCHAR2(50 BYTE), 
	"INFORMATION" VARCHAR2(50 BYTE)
) SEGMENT CREATION DEFERRED 
PCTFREE 10 PCTUSED 40 INITRANS 1 MAXTRANS 255 
NOCOMPRESS LOGGING
TABLESPACE "hostspace" ;

#### 3.创建视图
- 创建基于timetable表的视图v1
 CREATE OR REPLACE FORCE EDITIONABLE VIEW v1 ("ID") AS 
  SELECT id FROM timetable;
  
- 创建基于students表的视图v2，查询性别为女的学生信息
create view v2
as 
select id,name,class_id,age,sex from students where sex = '女';

#### 4.向数据库中插入数据
- 学生表
DECLARE
    maxnumber   CONSTANT NUMBER := 50000;
    i           NUMBER := 1;
    my_class_id NUMBER := 1;
BEGIN
    FOR i IN 1..maxnumber LOOP
    if mod(i,500)=0 then my_class_id:=my_class_id+1;
    end if;
        INSERT INTO students ( id,name,class_id,age,sex ) VALUES (
            i,
            concat('student_name_',i),
            my_class_id,
            20,
            'man'
        ); --CONCAT('test',i)是将test与i进行拼接

    END LOOP;
    COMMIT;
END;

- 课程表
DECLARE
    maxnumber   CONSTANT NUMBER := 100;
    i           NUMBER := 1;
BEGIN
    FOR i IN 1..maxnumber LOOP
        INSERT INTO course ( id,name,credit ) VALUES (
            i,
            concat('subject_name_',i),
            4
        ); 

    END LOOP;
    COMMIT;
END;

- 课表信息表
DECLARE
    maxnumber   CONSTANT NUMBER := 50000;
    i           NUMBER := 1;
    my_timetable_id NUMBER := 1;
    my_class_id NUMBER := 1;
    my_teacher_id NUMBER := 1;
BEGIN
    FOR i IN 1..maxnumber LOOP
    if mod(i,10)=0 then my_class_id:=my_class_id+1;
    end if;
    if mod(i,10)=0 then my_timetable_id:=my_timetable_id+1;
    end if;
    if mod(i,10)=0 then my_teacher_id:=my_teacher_id+1;
    end if;
        INSERT INTO timetable ( id,timetable_id,class_id,teacher_id ) VALUES (
            i,
            my_timetable_id,
            my_class_id,
            my_teacher_id
        ); 

    END LOOP;
    COMMIT;
END;

- 班级表
DECLARE
    maxnumber   CONSTANT NUMBER := 100;
    i           NUMBER := 1;
BEGIN
    FOR i IN 1..maxnumber LOOP
        INSERT INTO class ( id,name ) VALUES (
            i,
            concat('class_',i)
        ); 
    END LOOP;
    COMMIT;
END;

- 教师表
DECLARE
    maxnumber   CONSTANT NUMBER := 100;
    i           NUMBER := 1;
BEGIN
    FOR i IN 1..maxnumber LOOP
        INSERT INTO teachers ( id,name,information ) VALUES (
            i,
            concat('name1_',i),
            concat('information1_',i)
        ); 

    END LOOP;
    COMMIT;
END;

#### 5.PL/SQL语句
- 查询学生的id是否小于0，如小于就触发异常并输出
declare
student_ids i1 is select id from students where id<1;
one students.id%type;
e1 exception;
begin
open i1;
fetch i1 into one;
if i1%found then raise e1;
end if;
exception
when e1 then
dbms_output.put_line(one||'不正确');
close i1;
end;
/

- 查询班级人数
create or replace PACKAGE class_numbers IS

  PROCEDURE numberOfQueries(my_class_id NUMBER);
 END MyPack;
 /
 create or replace PACKAGE BODY class_numbers IS
   PROCEDURE numberOfQueries(my_class_id NUMBER)
   AS
   lft integer; 
   BEGIN
     select id into lft
     from students
     where class_id=my_class_id;
   COMMIT;
   END numberOfQueries;
 END class_numbers;

 set serveroutput on
 DECLARE
  my_class_id NUMBER;  
 BEGIN
  my_class_id := 1;
  class_numbers.numberOfQueries (my_class_id) ; 
 END;

 #### 6.函数
 - 创建一个函数获取学科总学分
 create or replace PACKAGE MyPack IS
 
  FUNCTION Get_AllCredit(V_DEPARTMENT_ID NUMBER) RETURN NUMBER;
  
 END MyPack;
 
 FUNCTION Get_AllCredit(V_DEPARTMENT_ID NUMBER) RETURN NUMBER
  AS
   N NUMBER(20,2);
   BEGIN
    SELECT SUM(subject.credit) into N FROM subject;
    RETURN N;
   END;
 END MyPack;

#### 7.创建存储过程
- 创建get_student_info
create or replace procedure get_student_info
(s_no number)
as
s_id number;
s_name varchar2(50);
s_class_id number;
s_age number;
s_sex varchar2(50)
begin
select id,name,class_id,age,sex
into s_id,s_name,s_class_id,s_age,s_sex
from students where id=s_no;
DBMS_OUTPUT.PUT_LINE('学生id：'||s_id);
DBMS_OUTPUT.PUT_LINE('学生姓名：'||s_name);
DBMS_OUTPUT.PUT_LINE('班级Id：'||s_class_id);
DBMS_OUTPUT.PUT_LINE('学生年龄：'||s_age);
DBMS_OUTPUT.PUT_LINE('学生性别：'||s_sex);
end get_student_info;
/

#### 8.数据库备份
- 第1步，为目录创建一个表空间
Create tablespace tools datafile ‘fielname’ size 500M;
- 第2步，创建USERS用户
Create user USERS identified by RMAN default tablespace tools temporary tablespace temp;
- 第3步，给USERS赋予权限
Grant connect , resource , recovery_catalog_owner to USERS;
- 第4步，打开USERS,连接数据库
$>USERS
USERS>connect catalog users/users
- 第5步，创建恢复目录
USERS>Create catalog tablespace tools
- 第6步，注册目标数据库
$>USEERS target internal/password catalog USERS/USERS@rcdb;
USERS>Register database;

- 全数据库备份
[oracle@oracle-pc ~]
$ cat rman_level0.sh
[oracle@oracle-pc ~]$ ./rman_level0.sh

- **查询备份文件**
[oracle@oracle-pc ~]$ cd rman_backup
[oracle@oracle-pc rman_backup]$ ll

- 查看备份文件的内容
[oracle@oracle-pc rman_backup]$ rman target /
RMAN> list backup;

- 删除数据库文件
rm /home/oracle/app/oracle/oradata/orcl/pdborcl/SAMPLE_SCHEMA_users01.dbf

- 数据库启动到mount状态
oracle@oracle-pc ~]$ sqlplus / as sysdba
SQL> shutdown immediate
SQL> shutdown abort
SQL> startup mount
SQL> exit

- 开始恢复数据库
[oracle@oracle-pc ~]$ rman target /
RMAN> restore database ;
RMAN> recover database;
RMAN> alter database open;
Statement processed
RMAN> exit

- 查看数据库是否恢复
[oracle@oracle-pc ~]$ sqlplus study/123@pdborcl
 SQL> select * from t1;

### 四、使用两台主机，通过DataGuard实现数据库整体的异地备份
#### 1.备用的数据库
mkdir -p /home/oracle/app/oracle/diag/orcl
mkdir -p /home/oracle/app/oracle/oradata/stdorcl/
mkdir -p /home/oracle/app/oracle/oradata/stdorcl/pdborcl
mkdir -p /home/oracle/arch
mkdir -p /home/oracle/rman
mkdir -p /home/oracle/app/oracle/oradata/stdorcl/pdbseed/
mkdir -p /home/oracle/app/oracle/oradata/stdorcl/pdb/
$sqlplus / as sysdba    

#### 2.主要的数据库       
alter database add standby logfile  group 5 '/home/oracle/app/oracle/oradata/orcl/stdredo1.log' size 50m; //sql>
alter database add standby logfile  group 6 '/home/oracle/app/oracle/oradata/orcl/stdredo2.log' size 50m; //sql>
alter database add standby logfile  group 7 '/home/oracle/app/oracle/oradata/orcl/stdredo3.log' size 50m; //sql> 
alter database add standby logfile  group 8 '/home/oracle/app/oracle/oradata/orcl/stdredo4.log' size 50m; //sql>

