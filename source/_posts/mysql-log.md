---
title: mysql8.0踩坑
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

**mysql连接出现 `cryptography is required for sha256_password or caching_sha2_password`**  
1. 参考 [mysql报错RuntimeError: cryptography is required for sha256_password or caching_sha2_p](https://blog.csdn.net/p_xiaobai/article/details/85334875)  
2. `ALTER USER 'root'@'localhost' IDENTIFIED BY 'password' PASSWORD EXPIRE NEVER;` #修改加密规则   
3. ` ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';` #更新一下用户的密码 
4. `FLUSH PRIVILEGES;` #刷新权限
5. `alter user 'root'@'localhost' identified by '123456';`  # 再次重置密码
6. 重启服务,问题解决