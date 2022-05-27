---
title: Pycharm的Settings中Python Interpreter一片空白nothing to show，并且没有任何选项可以选择解释器
top: false
cover: false
toc: true
mathjax: true
date: 2021-09-16 17:53:27
password:
summary:
tags: Pycharm python
categories: 软件工具
---
解决方法：

关掉Pycharm，删除项目文件夹下的.idea和Scripts文件夹，再重新打开Pycharm。右下角出现：

![Image text](https://img-blog.csdnimg.cn/20210713114426422.png)

 点击之后出现：

![Image text](https://img-blog.csdnimg.cn/20210713114602616.png)

选择Add Interpreter...之后就可以重新选择解释器了，Settings那里也恢复正常了

![Image text](https://img-blog.csdnimg.cn/20210713114715643.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzU4ODI3Mzk5,size_16,color_FFFFFF,t_70)

 尝试下右键run，一切正常。
、
