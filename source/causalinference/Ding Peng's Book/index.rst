.. vim: syntax=rst


.. highlight:: sh

Ding Peng's Book
=====================

本学期上了（2024秋）中山大学的蒋智超教授的因果推断课程，参考书目为丁鹏的《A First Course in Causal Infernece》，此处整理了该门课程六次课后作业的解答，欢迎交流

课后作业整体以课后习题为主，另外蒋教授也补充了另外的内容，包括辛普森悖论模拟，观察性实验中的ATT双稳健估计，必备知识证明等。

本人目前在中山大学读统计学方向硕士，邮件地址为：heyu58@mail2.sysu.edu.cn

关于如何在Sphinx中导入md文档，最新的版本推荐使用myst_parser，已经不在再使用recommonmark
::

   pip install myst_parser

  
conf.py文件中添加
::

   extensions = ['myst_parser']


.. toctree::
   :maxdepth: 1
   :titlesonly:

   DP_homework1
   DP_homework2
   DP_homework3
   DP_homework4
   DP_homework5
   DP_homework6




