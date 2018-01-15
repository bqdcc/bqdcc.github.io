---
title: PM2
date: 2017-11-13 20:56:24
tags: node 
---

## [PM2](http://pm2.keymetrics.io/docs/usage/cluster-mode/)

$ pm2 start app.js -i 4 # cluster mode 模式启动4个app.js的应用实例  4个应用程序会自动进行负载均衡

                           
$ pm2 start app.js --name="api" # 启动应用程序并命名为 "api"
$ pm2 start app.js --watch # 当文件变化时自动重启应用

$ pm2 list # 列表 PM2 启动的所有的应用程序
$ pm2 monit # 显示每个应用程序的CPU和内存占用情况
$ pm2 show [app-name] # 显示应用程序的所有信息

<!--more-->

$ pm2 logs # 显示所有应用程序的日志
$ pm2 logs [app-name] # 显示指定应用程序的日志

$ pm2 stop all # 停止所有的应用程序
$ pm2 stop 0 # 停止 id为 0的指定应用程序
$ pm2 restart all # 重启所有应用
$ pm2 reload all # 重启 cluster mode下的所有应用

$ pm2 delete all # 关闭并删除所有应用
$ pm2 delete 0 # 删除指定应用 id 0
$ pm2 scale api 10 # 把名字叫api的应用扩展到10个实例


pm2 远程部署 需要创建 ecsystem.json
>		{
			"apps": [
				{
					"name": "应用名称",
					"script": "./bin/www", #应用
					"watch": true,  #是否开启自动重启
					"env": {
						"COMMON_VARIABLE": "true"
					},
					"env_production": {
						"NODE_ENV": "production"
					}
				}
			],
			"deploy": {
				"production": {
					"user": "***", #服务器用户名
					"host": [
						"ip地址"
					],
					"port": "ssh端口号",
					"ref": "origin/master",
					"repo": "git仓库地址",
					"path": "服务器部署路径",
					"ssh_options": "StrictHostKeyChecking=no",
					"post-deploy": "yarn install && pm2 startOrRestart ecosystem.json --env production", #要执行操作 此处是下载依赖 启动应用
					"env": {
						"NODE_ENV": "production"
					}
				}
			}
		}

pm2 deploy ecosystem.json production setup #远程部署
pm2 deploy ecosystem.json production       #远程更新 并执行 post-deploy 中的操作