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