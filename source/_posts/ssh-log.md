---
title: 记录一次服务器免密登录失效问题
date: 2019-12-19 10:32:57
tags:
    - 日志
    - 踩坑记
---

1. 在本地使用`ssh-keygen`生成密钥  
`ssh-keygen -t rsa`

2. 将本地`~/.ssh/id_rsa.pub`使用`ssh-copy-id`复制到`user@hostname`下  
`ssh-copy-id -i ~/.ssh/id_rsa.pub user@hostname`

3. 成功后我尝试使用`ssh user@hostname`连接发现还是需要输入密码,随后检查了半天,随后去了百度参考了[远程登陆不要密码，使用authorized_keys不生效的解决方法](https://blog.csdn.net/huang_xw/article/details/8675132)才知道是我的`.ssh`文件夹与`authorized_keys`文件权限错误的原因, 随后修改文件夹权限再次连接,成功解决!  

服务端`~/.ssh`文件夹权限必须是 **`700`**  
    `chmod 700 ~/.ssh`  
服务端`~/.ssh/authorized_keys`权限必须是 **`600`**  
    `chmod 600 ~/.ssh/autohrized_keys`  
