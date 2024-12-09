.. vim: syntax=rst


.. highlight:: sh

Oracle
=====================

本学期（2024春）选修了中山大学黄志洪老师的《数据库原理与应用》课程，以Oracle环境为主授课。此处整理一部分习题，欢迎交流学习。


环境变量永久化设置
-------------------

::

   set linesize 120
   set pagesize 90
   set heading off


SQL初级语句（以emp表为例子）
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
    #（测试通配符%与_的区别）

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
    #(可直接对视图修改)

给 EMP 表增加一列 LOC，然后记录每位员工所在城市

::

    update emp set loc=(select loc from dept where deptno=emp.deptno)


利用控制文件导入外部数据
-------------------------------
假设我们有一个txt文件（000001.SS）需要导入数据库，其位置为C：\Temp

::
    
    Date,Open,High,Low,Close,Adj Close,Volume
    1997/7/2,1255.909058,1261.571045,1147.331055,1199.061035,1199.061035,0
    1997/7/3,1194.676025,1194.676025,1149.939941,1150.623047,1150.623047,0
    1997/7/4,1138.921021,1163.249023,1124.776001,1159.342041,1159.342041,0
    1997/7/7,1161.707031,1163.447021,1085.572021,1096.81897,1096.81897,0
    ...#存放的上证指数数据在之后的分析函数中会用到

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

