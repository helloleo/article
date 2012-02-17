---
title: 使用nginx、supervisor、tornodo部署flask应用  
date: 2012-02-17  
category: Code

---
对于flask引用的部署方式一直比较迷惑，尝试过了fcgi和uwsgi的方式来部署，最后还是决定使用supervisor启动多个包裹着flask应用的tornado应用进程的方法来部署，有点绕哈。这样做的好处是可以通过supervisor来逐个重启每个进程，保证不会因为更新代码而导致暂时不可访问。

思路就是使用tornado来启动flask应用；supervisor来管理守护tornado进程；nginx来做服务器前端负载均衡。

测试环境是debian 6、nginx 1.0.12、supervisor 3.0a8、flask0.8。  

tornado服务app代码：(tornado_server.py)

	#!/usr/bin/env python
	# coding:utf-8
	
	import sys
	from tornado.wsgi import WSGIContainer
	from tornado.httpserver import HTTPServer
	from tornado.ioloop import IOLoop
	from yourapp import yourapp #引入flask应用
	
	http_server = HTTPServer(WSGIContainer(app))
	http_server.listen(int(sys.argv[1]))
	IOLoop.instance().start()
	
spuervisord.conf 配置，该文件位于/etc/supervisor/supervisord.conf

	[group:testapp]
	programs=testapp
	
	[program:testweb]
	command=python /home/username/appdir/tornado-server.py 80%(process_num)02d
	process_name=%(program_name)s-80%(process_num)02d ;这句不可缺少
	directory=/home/username/appdir/
	autorestart=true
	redirect_stderr=true
	stdout_logfile=/home/username/appdir/logs/tornado-server-80%(process_num)02d.log ;这个文件不能为空
	stdout_logfile_maxbytes=500MB
	stdout_logfile_backups=50
	stdout_capture_maxbytes=1MB
	stdout_events_enabled=false
	loglevel=warn
	numprocs=4 ;这里是启动的进程数
	numprocs_start=1

在/etc/nginx/site-available下新建一个配置文件命名为tornado_conf，内容为：

	upstream testserver {
		server 127.0.0.1:8001;
		server 127.0.0.1:8002;
		server 127.0.0.1:8003;
		server 127.0.0.1:8004;
	}
	server {
		listen 80;
		server_name www.test.com;
		charset utf-8;
		location / {
			proxy_pass http://testserver;
			proxy_set_header X-Real-IP $remote_addr;
			proxy_pass_header Set-Cookie;
		}
	}

使用ln -s 命令在/etc/nginx/site-enabled/文件夹下建立tornado_conf的软链接。

启动nginx：

	/etc/init.d/nginx start

重新加载配置文件：

	nginx -s reload

启动supervisor：

	/etc/init.d/supervisor start

重新加载配置文件：

	supervisorctl reload

访问127.0.0.1应该可以看到自己的app在运行了。

---

资源链接：  
supervisor网站：[http://supervisord.org/](http://supervisord.org/)  
tornado网站：[http://www.tornadoweb.org/](http://www.tornadoweb.org/)  
flask网站：[http://flask.pocoo.org/](http://flask.pocoo.org/)

参考链接：  
[崔玉松](http://fendou.org) - [《使用supervisor和nginx发布tornado程序》](http://fendou.org/2011/09/23/supervisor-nginx-tornado/)  
[Dndx](http://www.idndx.com/) - [《Tornado + Supervisor 在生产环境下的部署方法》](http://www.idndx.com/posts/ways-to-deploy-tornado-under-production-environment-using-supervisor.html)  
[陈小玉](http://chenxiaoyu.org) - [《supervisor - Python进程管理工具》](http://chenxiaoyu.org/2011/05/31/python-supervisor.html)