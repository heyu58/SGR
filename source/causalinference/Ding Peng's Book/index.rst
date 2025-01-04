.. vim: syntax=rst


.. highlight:: sh

Ding Peng's Book
=====================

本学期上了（2024秋）中山大学的蒋智超教授的因果推断课程，参考书目为丁鹏的《A First Course in Causal Infernece》

国内本科暂时没有完整的因果推断课程，此处整理了该参考书目六次课后作业的解答，欢迎交流

本人目前在中山大学读统计学方向硕士，邮件地址为：heyu58@mail2.sysu.edu.cn

关于如何在Sphinx中导入md文档，最新的版本推荐使用myst_parser，已经不再使用recommonmark
::

   pip install myst_parser

  
conf.py文件中添加
::

   extensions = ['myst_parser']


.. toctree::
   :maxdepth: 1
   :titlesonly:
   
   DP_homework1.md
   DP_homework2.md
   DP_homework3.md
   DP_homework4.md
   DP_homework5.md
   DP_homework6.md




