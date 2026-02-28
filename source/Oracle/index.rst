.. vim: syntax=rst


.. highlight:: sh

数据库原理与应用
=====================

本学期（2024春）选修了中山大学黄志洪老师的《数据库原理与应用》课程，Oracle授课。此处整理一部分习题，欢迎交流学习。
黄志洪老师本人授课风格强烈，有大量一线业务经历，其创办的炼数成金网站也是良好的学习平台.

以下内容都是节选自其授课PPT中：

* Oracle三大难度之巅：with递归（N皇后问题），层次查询（员工信息传递最短路径问题），分析函数

* 什么是大数据？任何简单的场景，只要量上去了，都会变得非常复杂

* 为什么需要分布式集群（以提供可线性增长的计算力和存储能力）而不是巨型计算机？

* 数据库的核心问题：怎样存储（存储引擎），怎样快速定位（索引），怎样查询和维护（操作命令的解析，需要一整套语法固定的万能查询语言）

* Oracle官网: https://docs.oracle.com/en/database/oracle/oracledatabase/21/development.html

* 是否有完善的高可用、负载均衡、灾备能力是企业级数据库软件与开源软件的分水岭


环境变量永久化设置
-------------------

::

   show all --查看sqlplus的环境变量
   set linesize 120
   set pagesize 90
   set heading off


SQL初级语句（以emp表,deptno表为例）
--------------------

列出e工资在 2000-3000 之间（包括临界值）并且没有提成的员工

::
    
    select * from emp where sal between 2000 and 3000 and comm is null;

列出和JONES 同一部门的员工名字

::

    select ename from emp where deptno=(select deptno from emp where ename='JONES');

列出名字以 S 开头，名字长度为 5 个字符的员工

::

    select * from emp where ename like 'S____';
    --（测试通配符%与_的区别）

列出在芝加哥工作的员工，并且按工资从高到低排列 

::

    select * from emp where deptno=(select deptno from dept where loc='CHICAGO') order by sal desc;

显示员工的姓名和总收入，总收入以美元符号$开头，带小数点后两位 

::

    select ename,to_char(sal+nvl(comm,0),'$9999.99') from emp;

列出员工号和姓名以及所属部门名称，要求员工号用中文大写，例如“1234”输出为”壹贰叁肆“ 

::
     
    select translate(empno,'0123456789','零壹贰叁肆伍陆柒捌玖') as empno,ename,deptno from emp;

显示员工的姓名和雇佣时间，要求中文时间格式，即“2012年9月26日”这样的形式（要求不出现09月,把0去掉）

::

    select ename.replace(replace(to_char(hiredate,’yyyy”年”mm”月”dd”日”’),’年0’,’年’),’月0’,’月’)

假设每位员工在进入公司满10年后的第一个4月1日，会给他发放“优质服务奖”，请列出每位员工的获奖时间

::
    
    select ename,hiredate,to_char(hiredate,’yyyy’)+10+greatest(sign(to_char(hiredate,’mmdd’)-401),0)||’年4月1日’ from emp; 

列出工资从高到低排列的第 3-5 名员工

::  
    
    select * from(select rownum r,a.* from(select rownum,ename,sal from emp e order by sal desc) a) b where r<=5 and r>=3;

给在波士顿，纽约，芝加哥，达拉斯的员工分别加薪 300，500，380，210 美元（要求 1 条语句完成） 

::

    update (select * from emp natual join dept) set sal=sal+decode(loc,‘BOSTON’,300,‘NEW YORK’,500,‘CHICAGO’,380,‘DALLAS’,210);
    --(可直接对视图修改)

给 EMP 表增加一列 LOC，然后记录每位员工所在城市

::

    update emp set loc=(select loc from dept where deptno=emp.deptno)
    

把工资（从高到低）排名 10-12 名的员工加薪 300 美元（要求 1 条语句完成） 


::

     update emp set sal=sal+300 where ename in (
     select ename from (select rownum r,s.ename from 
     (select rownum,e.* from emp e order by sal) s) 
     where r between 10 and 12);


列出部门的名称和部门内员工的不同工种数

::
    
    select deptno,count(distinct job) from emp group by deptno;

求每年进入公司工作的员工数 

::

    select trunc(hiredate,'year'),count(distinct ename) from emp group by trunc(hiredate,'year');


用 1 条 SQL 语句建立以下统计表格，分别统计每个部门，每个年份进入公司，每个工种 的人数

::

    select deptno,to_char(hiredate,'yyyy'),job,count(distinct ename) from emp 
    group by rollup(deptno,to_char(hiredate,'yyyy'),job) 
    having grouping(job)<1;





利用控制文件导入外部数据
-------------------------------
假设我们有一个txt文件（000001.SS）需要导入数据库，其位置为C：\Temp

::
    
    Date,Open,High,Low,Close,Adj Close,Volume
    1997/7/2,1255.909058,1261.571045,1147.331055,1199.061035,1199.061035,0
    1997/7/3,1194.676025,1194.676025,1149.939941,1150.623047,1150.623047,0
    1997/7/4,1138.921021,1163.249023,1124.776001,1159.342041,1159.342041,0
    1997/7/7,1161.707031,1163.447021,1085.572021,1096.81897,1096.81897,0
    ...--存放的上证指数数据在之后的分析函数中会用到

我们在同样的位置C:\Temp创建一个名为ss的ctl控制文件,内容为：

::

    options(skip=1,rows=4096)
    load data
    infile "c:\TEMP\000001.SS.csv"
    truncate
    INTO table ss001
    fields terminated by ","
    (day,open,high,low,close,adjclose,volume)

只要数据库中已经存在ss001表（如何创建省略），我们在cmd控制台在C:\Temp目录下输入
::

    sqlldr control=ss.ctl errors=100000

即可成功导入

建表与约束
---------------------------------

3、创建以下 3 个表，要求所有的约束都要建立约束名
学生表S：学号，姓名，所属系，年龄。其中学号为主键，姓名要求非空约束
课程表C：课程号，课程名，先行课号。其中课程号为主键，课程名要求非空约束 
学生成绩表SC：学号，课程号，成绩。
（学号与课程号为联合主键，学号为外键，参照学生表的学号属性；课程号为外键，参照课程表的课程号属性。分数要求在 0-100 之间 ）

::

    create table S(
        S-- varchar2(5),
        SN varchar2(5) not null,
        SD varchar2(5),
        SA number(3),
        Primary key (S--));
    create table C(
        C-- varchar2(5),
        CN varchar2(5) not null,
        PC-- varchar2(5),
        primary key (C--));
    create table SC(
        S-- varchar2(5),
        C-- varchar2(5),
        G number(3) check (G between 0 and 100),
        primary key (S--,C--),
        constraint SC_FKS foreign key (S--) references S(S--),
        constraint SC_FKC foreign key (C--) references C(C--),
        constraint SC_CHECK check (G between 0 and 100));
    desc S--通过desc语句查看表


视图
---------------------------------
视图不保存实际数据，只保存生成视图的select语句，当访问视图时，select语句会被运行，从基表获得数据生成视图数据
* 对基表的修改会影响视图数据
* 对视图的修改会反映在基表上

列出公司中工资最高的头三名员工（可以用视图解决Top n问题）
::

    create view vtop as ......;
    select * from vtop where rownum<=3;
    drop view vtop; 


插入、修改和删除数据
---------------------------------
::

    --不同的插入数据方式
    insert into emp (empno,ename,hiredate) values (1110,'Jim','20-11月-10');
    insert into emp (empno,ename,hiredate) values (1110,'Jim',to_date('19901212100000','yyyymmddhh'));
    insert into emp select empno,ename from emp where job='SALESMAN';
    --修改数据
    update emp set sal=8000 where ename='Jim';
    --注意删除操作的高危型
    delete from emp where enmae = 'Jim';

**【注意】如果在delete语句后面忘记加where条件，将删除全部行！**


SQL中级语句(反选<>,exists,pivot,连接)
------------------------------

表 A 有 C1，C2 两列，分别记录了所有商品编号（唯一）和商品价格，表 B 也有 C1 和 C2 列，记录了部分商品（非全部）的新价格，请用 B 的数据更新 A 表中的商品价格

::
     
    --利用exists确保只更新与B中id匹配的行
    update A set sale=(select B.newsale from B where A.id=B.id) where exists (select 1 from B where A.id=B.id);

* 如果要更新的表中存在之前没有的变量

::
    
    Merge into A using B on (A.id=B.id) 
    when matched then update set A.id=B.id 
    when not matched then insert values(B.id,B.newsale)


在之前的学生选修表 SC 与课程表 C 放置一些数据，写一条 SQL 求出选修了 C 表所列全部课程 的学生名单

::
    
    --方法1：直接看选的课程个数
    select S--,count(*) from SC group by S-- having count(*)=(select count(*) from C);
    --方法2：SQL语句中的经典的“除法运算”
    select SN from S 
    where not exists (select * from C 
    where not exists (select * from SC where S--=S.S-- and C--=C.C--)
    );

* 在 SC 表中加入大量数据，然后用 pivot 函数将它转为宽表 SCwide。再用 unpivot 函数将 SCwide 转为窄表

::

    --窄表转宽表
    create table SCwide as select * from SC pivot (sum(G) for C-- in ('C1' C1,'C2' C2,'C3' C3,'C4' C4,'C5' C5));
    --宽表转窄表
    select * from SCwide unpivot(a for b in (C1,C2,C3,C4,C5));


列出emp表中员工的姓名与上司的姓名

::

    --自连接，利用表别名实现
    select a.ename,b.ename from emp a,emp b where a.mgr=b.empno;
    --外连接，分为左外连接、右外连接、全外连接
    select a.ename,b.ename from emp a,emp b where a.mgr=b.empno(+);
    select a.ename,b.ename from emp a left outer join emp b on a.mgr=b.empno;
    select a.ename,b.ename from emp a right outer join emp b on a.mgr=b.empno;
    select a.ename,b.ename from emp a full outer join emp b on a.mgr=b.empno;


聚组统计函数

::

    --求部门人数超过4个人的部门平均工资
    select deptno,avg(sal),count(*) from emp 
    group by deptno having count(*)>4;
    --列出平均工资超过公司平均工资的部门
    select dname,avg(sal) from emp natural join dept 
    group by dname
    having avg(sal)>(select avg(sal) from emp);
    --统计每年进入公司工作的员工数量
    select trunc(hiredate,'year'),count(distinct ename) from emp 
    group by trunc(hiredate,'year');


集合运算（Minus,intersect,union,union all）




索引
-----------------------------
索引自动维护，自动使用,主键，候选键对应的列会自动建立索引。一张表不适合建立太多索引，可以建在大表常用的查询条件上。
11g以后的版本由于大量使用内存缓冲数据，索引的效果对于不太大的表不是很明显（做实验时表的大小应超过内存数）

索引技术是数据库产品最重要的核心技术之一
::

    create index test on test_table(number)


用户、权限、角色、同义词、视图
---------------------------
* 用户（User）：数据库中的一个账号，每个用户都有自己的权限和角色。用户可以创建自己的表空间和数据库对象。
Oracle数据库在安装后会默认创建一些系统用户，如sys、system和scott等

* 权限（Privilege）：分为系统权限和对象权限
系统权限：允许用户执行特定的数据库操作，例如CREATE SESSION、CREATE TABLE等

对象权限：允许用户对特定数据库对象进行操作，例如SELECT、INSERT、UPDATE、DELETE等

* 同义词（Synonym）：是为数据库对象（如表、视图、序列等）创建的别名，允许用户忽略对象的所有者前缀，直接访问对象。
* 视图（View）：基于SQL查询的虚拟表，可以简化复杂的SQL操作，提高数据安全性。视图可以包含表的列或计算字段，用户可以像操作普通表一样对视图进行查询

::

    create user y1 identified by y1; --创建用户y1,密码y1
    grant connect to y1;  --授权可以连接到Oracle
    grant create synonym to y1; --授权创建公共同义词
    grant create view to y1; --授权创建视图
    grant select any table to y1;  --授权可以访问任意表
    

* 角色(Role):一组权限的集合，可以简化权限管理。
常见的角色包括:

CONNECT：允许用户连接到数据库并执行基本操作,如CREATE SESSION、CREATE SYNONYM、CREATE VIEW等

RESOURCE：允许用户创建自己的数据库对象,如表、序列、视图等

DBA：拥有所有系统权限，是数据库管理员角色

::

    create role y2;  --创建角色y2
    grant connect to y2; --为y2角色赋予连接权利

用户名public是oracle内置用户，相当于windows系统中“everyone”的作用
授予给public的权限（系统特权，对象特权，角色）可以被全体用户使用（包括未来才建立的用户）



数据字典
-----------------------------
* 数据字典视图：预先构建好的系统表的视图的公共同义词
每种视图具有特定的功能，例如查询表的信息，用户信息，权限信息，存储空间等。用户可以通过查询数据字典视图了解系统运行情况。
困难点在于要记住成百上千的视图名


列出用户拥有的表、列出用户拥有的表中的列、观看用户拥有的特定对象

::

    desc user_tables  --列出当前用户所拥有的表
    desc user_views  --列出当前用户拥有的视图
    desc user_sys_privs   --列出当前用户系统特权
    select username from dba_users   --列出所有用户（注意是否有权限）
    select * from user_sys_privs where username='SCOTT'  --列出SCOTT的所有权限（注意是否有权限）

查出系统最近三天创建的表

::

    select owner,object_name from dba_objects
    where object_type='TABLE'
    and created>sysdate-3




SQL高级语句（分析函数）
-------------------------------
可以理解为聚组统计的推广，但功能更加丰富，市面上关于分析函数的资料不多（可能太复杂不好理解）
从下面一个简单的例子出发开始学习分析函数，基本由关键词<over>，分区短语（partition by），排序短语（order by），开窗短语（rows/range between...）构成

::

    select empno,ename,deptno,hiredate,sal,
    avg(sal) over (partition by deptno order by hiredate) avg_sal,# 注意不需要as来命名
    min(sal) over (order by hiredate) min_sal
    sum(sal) over (partition by deptno order by hiredate) sum_sal,
    max(sal) over (partition by deptno order by hiredate) max_sal,
    count(sal) over (partition by job order by hiredate) count_sal
    from emp;

开窗短语用于指出在分区内的统计范围（基于数前后行数）无论怎样分区，以及怎样制定开窗，分析函数的统计范围都不超出所在分区以外。没有窗短语关键词时，缺省的关键词
为*rows between unbounded preceding and current row*

*rows between 1 preceding and current row*

*rows between 1 preceding and 3 preceding*

*rows between 2 preceding and 1 following*

::

    select empno,ename,deptno,hiredate,sal,
    sum(sal) over (partition by deptno order by hiredate
    range between unbounded preceding and 365 following) sum_sal
    from emp;

*range between unbounded preceding and 365 following*从本分区最开始的行一直累加到hiredate+365（值范围）的行

* 常用的分析函数包括：

* 1、统计函数：avg,max,min,sum,count

* 2、排序函数：RANK（有并列1、2、2、4、4、6），DENSE_RANK（有并列1、2、2、3、3、4），FIRST，FIRST_VALUE，LAST，LAST_VALUE，LAG，LEAD，ROW_NUMBER（无并列）

* 3、数据分布函数：cume_dist，Percent_rank

* 4、统计分析函数：


PL/SQL存储函数
-----------------------------
中国传统使用“天干地支纪年法”，天干依次为“甲、乙、丙、丁、戊、己、庚、辛、壬、癸”10种，地支依次为“子、丑、寅、卯、辰、巳、午、未、申、酉、戌、亥”12 种。
例如 2022 年是壬寅年，则 2023 年是癸卯年，2024 年是甲辰年等如此类推，每 60 年完成一次循环。
要求用 PL/SQL 实现存储函数，输入公元纪年（正整数，不要求考虑公元前的情况），输出干支纪年（简体中文）。

::

    --select * from sys.user_errors where name=upper('y');
    --利用上面语句更准确的查看错误点
    create or replace function y(n number)
    return varchar2 as 
    T1 varchar2(20):='甲乙丙丁戊己庚辛壬癸';
    T2 varchar2(24):='子丑寅卯辰巳午未申酉戌亥';
    N1 number(4):=0;
    N2 number(4):=0;
    begin
    select decode(mod((n-3),10),0,10,mod((n-3),10)) into N1 from dual;
    select decode(mod((n-3),12),0,12,mod((n-3),12)) into N2 from dual;
    return substr(T1,N1,1)||substr(T2,N2,1)||'年';
    end;

例外Exception
-----------------------------
当在 SC 表中插入的行包含 S 表中不存在的学号时，会发生外键引用错误，请写一段 PLSQL程序进行测试，建立例外处理捕捉此类错误并输出预先定义的警告信息

::

    DECLARE
    t exception; 
    pragma exception_init(t,-2291);
    --ORA-02291: 违反完整约束条件 (SCOTT.SC_FKS) - 未找到父项关键字
    BEGIN 
    insert into SC(S--,C--,G) values('S7','C1',100);
    EXCEPTION 
    WHEN t
    Then dbms_output.put_line('该学号不存在'); 
    END;
    /


游标
------------------------------
之前的作业导入了“上证指数历史数据”，请写一段 PLSQL 代码，用建立游标的方法，找出所有“连升三天”“连跌三天”的日期 

::
    
    DECLARE
        cursor cur IS SELECT to_date(day,'yyyy/mm/dd') as day, close FROM SS001 ORDER BY day;
        sal_today   NUMBER;
        sal_yesterday NUMBER;
        sal_day_before_yesterday NUMBER;-- 定义变量来存储结果集
        daystart date;
        dayend date;
        cur_row cur%rowtype;-- 游标类型变量
    BEGIN
        open cur;
        loop
        FETCH cur INTO cur_row;
        EXIT WHEN cur%NOTFOUND;  --直到游标抓取为空
        IF cur%ROWCOUNT = 1 THEN
            sal_today := cur_row.close;
        ELSIF cur%ROWCOUNT =2 THEN
            sal_yesterday:=sal_today; 
            sal_today:=cur_row.close;
        -- 检查是否是前两行数据
        ELSE
            sal_day_before_yesterday := sal_yesterday;
            sal_yesterday := sal_today;
            sal_today := cur_row.close;
            -- 检查连升三天
            IF sal_today>sal_yesterday AND sal_yesterday>sal_day_before_yesterday THEN
                dayend := cur_row.day;
                daystart:=dayend-3;
                DBMS_OUTPUT.PUT_LINE
                ('芜湖连升三天: '||to_char(daystart,'yyyy-mm-dd')||'到'||to_char(dayend,'yyyy-mm-dd'));
            END IF;
            -- 检查连跌三天
            IF sal_today<sal_yesterday AND sal_yesterday<sal_day_before_yesterday THEN
                dayend := cur_row.day;
                daystart:=dayend-3;
                DBMS_OUTPUT.PUT_LINE
                ('我靠连跌三天:'||to_char(daystart,'yyyy-mm-dd')||'到'||to_char(dayend,'yyyy-mm-dd'));
            END IF;
        END IF;
        END LOOP;
        close cur;
    END;


触发器
-------------------------------
Oracle触发器是Oracle数据库中一种特殊的存储过程，它能够在特定数据库事件发生时自动执行预定义的操作

写一个触发器，使emp表只有在周一到周五8:00-18:00这个时间段才可以被修改

::

    create or replace trigger s_emp
    before insert on emp
    begin
    if (to_char(sysdate,'DY') in ('星期六','星期日') 
    or (to_char(sysdate,'HH24:MI') not between 
    '08:00' and '18:00'))
    then raise_application_error(-20500,'有人入侵');
    end if;
    end;

写一个触发器，用于记录所有的修改记录（审计需求）

::


    create table rec (name varchar2(40),time date);
    --创建一个表rec用于记录修改情况

    create or replace trigger rec_update
    after update on emp
    begin
    insert into rec values(user,sysdate);
    end;



写一个触发器，禁止除了boss以外的人工资记录超过5000

::

    create or replace trigger r_sal
    before insert or update of sal on emp for each row
    begin
    if not (:new.ename='KING') and :new.sal>=5000
    then raise_application_error(-20202,'非法工资');
    end if;
    end;


触发器的合理应用有助于保持数据一致性和安全性，但过多触发器会导致调试复杂，影响性能（点火顺序问题）

::

    --禁用触发器 
    alter trigger tri_uname(触发器名字) disable;
    --激活触发器 
    alter trigger tri_uname(触发器名字) enable;
    --重新编译 
    alter trigger tri_uname(触发器名字) complie;
    --禁用某个表上的触发器 
    alter table table_name(表名) diable all triggers;
    --删除触发器 
    DROP TRIGGER tri_uname(触发器名字);


Oracle中DBMS包应用
-------------------------------

DBMS包是Oracle数据库提供的一系列预定义的包，涵盖了数据管理、系统管理、网络通信等多个方面。这些包不仅简化了复杂任务的实现，还提高了代码的可重用性和可维护性。常见的DBMS包包括：

DBMS_OUTPUT：用于输出调试信息。
DBMS_SQL：提供动态SQL执行功能。
DBMS_JOB：用于创建和管理定时任务。
DBMS_LOCK：提供锁定机制，用于并发控制。
DBMS_UTILITY：提供数据库管理和调试工具


使用DBMS_DDL包完成以下任务，批量创建99个用户A01-A99，口令均为 tiger，在这99位用户下都建立emp表并且把scott的emp表内容复制过去 

::
    
    DECLARE
        username varchar2(4);
    BEGIN
        for i in 1..99 loop
        username:='A'||LPAD(i,2,'0');
        execute immediate 'create user '||username||' identified by tiger';
        execute immediate 'grant resource,connect,dba to '||username;
        execute immediate 'create table '||username||'.emp as (select * from scott.emp)';
    end loop;
    END;

表空间信息查询
--------------------------------
求各表空间的容量（注意一个表空间对应多个数据文件的情况），剩余空间和使用率，要求一条 SQL 语句完成

::
     
    --DBA_FREE_SPACE freespace中记录了表空间中的自由段
    --DBA_DATA_FILES dat中记录了数据文件的相关信息
    --DBA_TABLESPACES ts中记录了表空间的相关信息
    WITH freespace as(
        --记录每个表空间的未分配空间
        select tablespace_name,sum(bytes) as fs
        from DBA_FREE_SPACE
        group by tablespace_name
    )
    SELECT ts.TABLESPACE_NAME，
    --记录每个表空间中文件实际占用的空间（未占满整个分配的空间）
        sum(dat.BYTES)/(1024*1024*1024) as dat_GB,
        sum(freespace.fs)/(1024*1024*1024) as free_GB,
        sum(dat.BYTES)/(sum(dat.BYTES)+sum(freespace.fs))*100 as usedPercent
    FROM 
        DBA_DATA_FILES dat
    JOIN 
        DBA_TABLESPACES ts ON dat.tablespace_name=ts.tablespace_name
    JOIN 
        freespace ON ts.tablespace_name=freespace.tablespace_name
    GROUP BY 
        ts.tablespace_name;


查出你建立的记录上证指数数据的ss001表位于哪个表空间？它的block数，extent数，以及每个extent的id，大小等详细信息

::
 
    CONNECT sys/sys as sysdba
    SELECT owner,tablespace_name,blocks,extent_id,bytes 
    FROM dba_extents where segment_name='SS001';




工作中关于SQL使用
-------------------------------

group by，order by, where,using()等语句的多条件匹配。

::
    --每天每个视频的访问次数
    SELECT 
        DATE(time) AS play_date,
        video_id,
        COUNT(*) AS play_count
    FROM t_video_play_action
    GROUP BY DATE(time), video_id

case when 条件1 and 条件2 then 值1 else 值2——语句多条件判断

::
    select *,
        CASE 
            WHEN income < 20000 THEN"Low Salary"
            WHEN income <= 50000 THEN"Average Salary"
            WHEN income > 50000 THEN"High Salary"
        END as category
    from Accounts

limit 限制显示(可结合order by来控制top问题)

::      

时间类型操作

::  
    ---日期加减
    DATE_ADD(table.recordDate,interval 1 day)
    DATE_SUB(table.recordDate,interval 1 day)

字符类型操作

::  
    ---字符串匹配查找电子邮件
    SELECT *
    FROM Users
    WHERE mail COLLATE utf8_bin REGEXP '^[a-zA-Z][a-zA-Z0-9_.-]*@leetcode\\.com$';