---
title: mongodb
date: 2017-11-17 14:39:45
tags: 数据库
---

[MongoDB](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/) ubuntu 系统安装

	安装时可使用阿里云镜像 
	镜像地址 ：http://mirrors.aliyun.com/mongodb/apt/ubuntu
	apt-get 镜像修改地址 ：/etc/apt/souces.list.d

<!--more-->

  mongodb日志存放位置 ：/var/log/mongodb/mongod.log

	mongodb配置文件路径 ：/etc/mongod.conf   ----- 端口号默认27017

	mongodb 指定端口连接 ：mongo --port 17777

	启动mongodb  ----------------  service mongod start
	停止mongodb  ----------------  service mongod stop
	重启mongodb  ----------------  service mongod restart

	数据库备份(导出) -- mongodump -h 127.0.0.1:27017 -d 数据库名称 -u 用户名 -p 密码 -o 备份的文件夹  //如果无用户名和密码可不加 -u 和 -p 参数

	压缩数据库 -- tar zcvf 数据库名称.tar.gz 数据库文件

	上传压缩包 -- scp -P 服务器ssh端口号 需要上传的文件 用户名@ip地址:/服务器放置文件路径

	下载     --  scp -P 服务器ssh端口号 用户名@ip地:服务器文件路径 本地路径

	解压压缩包 -- tar xvf tar包名称

	导入数据库 -- mongorestore -h 127.0.0.1:端口号 -d 数据库名称 -u 用户名 -p 密码 要导入的数据库文件夹路径

	切换数据库 -- use 数据库名称

	查看数据表 -- show tables

	查看表数据 -- db.表名称.find({})

	导出单表数据 -- mongoexport -d 数据库 -c 表名 -q '{数据规则}' -o 存储文件名.json  //无用户省略 -u -p

	导入单表数据 -- mongoimport --host 127.0.0.1:端口号 -d 数据库 -u 用户名 -p 密码 -c 表 导入文件路径

	删除数据库 -- mongo --host 127.0.0.1:端口号 数据库 --eval "db.dropDatabase()"

	查看数据 -- show dbs

	添加管理员用户 -- use admin
									 //此处创建的是一个超级管理员
									 //role 声明权限 db 声明为那个数据的用户 
									 db.createUser({user: 'admin',pwd: 'admin',roles: [{role: 'userAdminAnyDatabase',db: 'admin'}]})  
								   //role ：
											数据库用户角色（Database User Roles）：
												read：授予User只读数据的权限
												readWrite：授予User读写数据的权限

											数据库管理角色（Database Administration Roles）：
												dbAdmin：在当前dB中执行管理操作
												dbOwner：在当前DB中执行任意操作
												userAdmin：在当前DB中管理User

											备份和还原角色（Backup and Restoration Roles）：
												backup
												restore

											跨库角色（All-Database Roles）：
												readAnyDatabase：授予在所有数据库上读取数据的权限
												readWriteAnyDatabase：授予在所有数据库上读写数据的权限
												userAdminAnyDatabase：授予在所有数据库上管理User的权限
												dbAdminAnyDatabase：授予管理所有数据库的权限

											集群管理角色（Cluster Administration Roles）：
												clusterAdmin：授予管理集群的最高权限
												clusterManager：授予管理和监控集群的权限，A user with this role can access the config and local databases, which are used in sharding and replication, respectively.
												clusterMonitor：授予监控集群的权限，对监控工具具有readonly的权限
												hostManager：管理Server

	授权     --      db.auth('admin','admin')

	为其他数据库创建用户 需要现在`admin`数据库中授权 再创建

	开启用户验证 -- 修改配置文件 /etc/mongod.conf  修改后请重启 mongodb
								 security:
  							   authorization: 'enabled'
  
  指定用户打开指定数据库 -- mongo 127.0.0.1:端口号/数据库 -u 用户名 -p 密码