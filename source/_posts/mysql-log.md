---
title: mysql8.0修改密码问题
date: 2019-12-19 10:53:06
tags:
    - 日志
    - 踩坑记
---
在服务器中安装`mysql8.0`数据库修改密码发现无法使用`update`语句修改,于是百度了一番参考了[MYSQL8.0以上版本正确修改ROOT密码](https://blog.csdn.net/yi247630676/article/details/80352655)由此问题解决  
**解决步骤**:  
    1. 使用`mysql`初始生成的密码登录到数据库(密码在数据库产生的日志文件中, 可查看/`etc/my.cnf`中的`log-error`指向的文件)  
    2. 登录到数据库 `mysql -uroot -p'首次初始化数据库产生的密码'`  
    3. 登录执行 `alter user 'root'@localhost identified by 'new password';`  
    4. 执行成功后刷新数据库 `flush privileges;`问题成功解决  
