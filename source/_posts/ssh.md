---
title: ssh
date: 2017-11-13 21:08:53
tags: git
---

在根目录下新建
mkdir .ssh
生成 ssh 密钥
ssh-keygen -t rsa -b 4096 -C "邮箱"
查看 密钥 文件
cat id_rsa  #私钥
cat id_rsa.pub #公钥
开启 ssh代理
eval "$(ssh-agent -s)"
将ssh key 加入代理
ssh-add ~/.ssh/id_rsa
在.ssh目录下创建
vi authorized_keys 
将 本地 的 id_rsa.pub 公钥复制到 服务器的 authorized_keys 文件中 即可实现无密码登陆
vi /etc/ssh/sshd_config  linux 上 ssh配置 文件

ssh连接服务器 -- ssh -p 端口号 用户名@ip地址

上传文件 -- scp -P 服务器ssh端口号 需要上传的文件 用户名@ip地址:/服务器放置文件路径
压缩文件 -- tar zcvf 压缩包名.tar.gz 将要压缩的文件
解压压缩包 -- tar xvf 压缩包名称