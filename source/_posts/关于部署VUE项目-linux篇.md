---
title: 关于部署VUE项目--linux篇
top: false
cover: false
toc: true
mathjax: true
date: 2021-09-28 10:31:25
password:
summary:
tags: linux nginx vue
categories: 系统
---

1、首先，准备好两个工具，xshell、filezilla

第一个工具是为了连接Linux，
第二个工具是为了比较方便的上传文件到Linux。

2、使用xshell连接的时候。填上对应的地址就行。然后接下来就是点确实啥的，连上去就行。
![Image text](https://image-static.segmentfault.com/231/745/2317455061-5dcd2bd6d421e_fix732)
3、连接成功后，基本上都差补多，然后就是有个#号
![Image text](https://image-static.segmentfault.com/230/287/2302874803-5dcd2ca5ae6b9_fix732)

4、初次安装的nginx的忽略本步骤.

```
4.1 删除nginx与其配置文件
    sudo apt-get --purge remove nginx
4.2 自动移除全部不适用的软件包
    sudo apt-get autoremove
4.3 列出与nginx相关的软件
    dpkg --get-selections|grep nginx
    假如有以下的：
        nginx                       install
        nginx-common                    install
        nginx-core                  install
4.4删除在4.3中查询出与nginx有关的软件
    sudo apt-get --purge remove nginx
    sudo apt-get --purge remove nginx-common
    sudo apt-get --purge remove nginx-core
4.5 查看nginx正在运行的进程，如果有就kill掉
    ps -ef |grep nginx
4.6.kill nginx进程
    sudo kill -9 -8  //注释-9 -8进程id
4.7.全局查找与nginx相关的文件
    sudo  find  /  -name  nginx*
4.8 全部删除
    sudo rm -rf file
4.9 重新安装
    sudo apt-get update
    sudo apt-get install nginx
```

5、nginx的使用

```
1.切换到nginx的配置文件夹的目录下
cd /etc/nginx/conf.d

2.给对应的网站的配置文件，命名规则：项目名.conf
其实没啥要求主要是为自己好看，然后尾缀为conf.

3.对配置2进行添加配置参数
sudo vim 你在第二步中写项目名.conf
```

6、nginx的conf的主要内容

![Image text](https://image-static.segmentfault.com/367/963/3679631775-5dcd331627c9d_fix732)

注意：在每行语句的结束的地方要加上英文的 ;

```

server {
    listen 端口号;
    server_name IP地址;
    location / {
            root  /var/www/html/gameSystem; // 文件路径

            try_files $uri $uri/ @router; 
            
             index  index.html index.htm;  // 主页面
    }
    location /mgr {
    
            proxy_pass http:XXXX;  // 后台地址
    }
    location @router {
    
        rewrite ^.*$ /index.html last;
    }
}

```

7、好了，保存退出。对于vim的操作，百度一下吧

8、重启服务器：

service nginx restart
