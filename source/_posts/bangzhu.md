---
title: article title
top: false
cover: false
toc: true
mathjax: true
date: 2021-09-16 11:02:02
password:
summary:
tags:
categories:
---
写文章、发布文章
首先在博客根目录下右键打开git bash，安装一个扩展npm i hexo-deployer-git。

然后输入hexo new post "article title"，新建一篇文章。

然后打开D:\study\program\blog\source\_posts的目录，可以发现下面多了一个文件夹和一个.md文件，一个用来存放你的图片等数据，另一个就是你的文章文件啦。

编写完markdown文件后，根目录下输入hexo g生成静态网页，然后输入hexo s可以本地预览效果，最后输入hexo d上传到github上。这时打开你的github.io主页就能看到发布的文章啦。


来源: 韦阳的博客
作者: 韦阳
链接: https://godweiyang.com/2018/04/13/hexo-blog/
本文章著作权归作者所有，任何形式的转载都请注明出处。