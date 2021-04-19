

# 一、Nginx的SSL模块

**nginx: [emerg\] the "ssl" parameter requires ngx_http_ssl_module in /usr/local/nginx/conf/nginx.conf:37**

## 1.1 Nginx如果未开启SSL模块，配置Https时提示错误

**原因也很简单，nginx缺少http_ssl_module模块，编译安装的时候带上--with-http_ssl_module配置就行了，但是现在的情况是我的nginx已经安装过了，怎么添加模块，其实也很简单，往下看： 做个说明：我的nginx的安装目录是/usr/local/nginx这个目录，我的源码包在/usr/local/src/nginx-1.6.2目录**

```bash
nginx: [emerg] the ``"ssl"` `parameter requires ngx_http_ssl_module ``in` `/usr/local/nginx/conf/nginx.conf:37
```

### 1.2 Nginx开启SSL模块

切换到源码包：

```bash
cd /usr/local/src/nginx-1.11.3
```

查看nginx原有的模块

```bash
/usr/local/nginx/sbin/nginx -V
```

在configure arguments:后面显示的原有的configure参数如下：

```bash
--prefix=/usr/local/nginx --with-http_stub_status_module
```

那么我们的新配置信息就应该这样写：

```bash
./configure --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module
```

运行上面的命令即可，等配置完

配置完成后，运行命令

```bash
make
```

这里不要进行make install，否则就是覆盖安装

然后备份原有已安装好的nginx

```bash
cp /usr/local/nginx/sbin/nginx /usr/local/nginx/sbin/nginx.bak
```

然后将刚刚编译好的nginx覆盖掉原有的nginx（这个时候nginx要停止状态）

```bash
cp ./objs/nginx /usr/local/nginx/sbin/
```

然后启动nginx，仍可以通过命令查看是否已经加入成功

```bash
/usr/local/nginx/sbin/nginx -V　
```

## Nginx 配置Http和Https共存

```conf
server {``      ``listen 80 ``default` `backlog=2048;``      ``listen 443 ssl;``      ``server_name wosign.com;``      ``root /``var``/www/html;`` ` `      ``ssl_certificate /usr/local/Tengine/sslcrt/ wosign.com.crt;``      ``ssl_certificate_key /usr/local/Tengine/sslcrt/ wosign.com .Key;``    ``}
```

把ssl on；这行去掉，ssl写在443端口后面。这样http和https的链接都可以用

## Nginx 配置SSL安全证书重启避免输入密码

可以用私钥来做这件事。生成一个解密的key文件，替代原来key文件。

```bash
openssl rsa -``in` `server.key -``out` `server.key.unsecure
```

## Nginx SSL性能调优

```bash
ssl_protocols TLSv1 TLSv1.1 TLSv1.2;``ssl_ciphers ECDHE-RSA-AES256-SHA384:AES256-SHA256:RC4:HIGH:!MD5:!aNULL:!eNULL:!NULL:!DH:!EDH:!AESGCM;``ssl_prefer_server_ciphers ``on``;``ssl_session_cache shared:SSL:10m;``ssl_session_timeout 10m;
```

~~~conf
#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       80;
        server_name  localhost;
	charset utf-8;
	 location /group1/M00/{
                ngx_fastdfs_module;
        }

	location / {
		root   html;
            	index  index.html index.htm;

	}
	

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
        #location ~ \.php$ {
        #    proxy_pass   http://127.0.0.1;
        #}

        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}

        # deny access to .htaccess files, if Apache's document root
        # concurs with nginx's one
        #
        #location ~ /\.ht {
        #    deny  all;
        #}
    }


    # another virtual host using mix of IP-, name-, and port-based configuration
    #
    #server {
    #    listen       8000;
    #    listen       somename:8080;
    #    server_name  somename  alias  another.alias;

    #    location / {
    #        root   html;
    #        index  index.html index.htm;
    #    }
    #}


    # HTTPS server
    #
    server {
        listen       443 ssl;
        server_name www.pyfeige.com;

        ssl_certificate      /usr/local/nginx/conf/1_www.pyfeige.com_bundle.crt;
        ssl_certificate_key /usr/local/nginx/conf/2_www.pyfeige.com.key;

        ssl_session_cache    shared:SSL:1m;
        ssl_session_timeout  5m;

        ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers  on;
	charset utf-8;
	client_max_body_size 75M;
	# 这里配置反向代理的项目
        location / {

            proxy_pass http://127.0.0.1:8080;     # spring boot 项目的端口号

            # 固定写法-------------
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
            proxy_set_header X-Forwarded-Port $server_port;
        }

	 location /group1/M00/{
                ngx_fastdfs_module;
        }
	location /static {
		alias /data/wwwroot/myproject/project/static;
	}
	location /media {
		alias /data/wwwroot/myproject/project/media;
	}
	location /python {
		uwsgi_pass 127.0.0.1:8001;
		include /usr/local/nginx/conf/uwsgi_params;
		root   html;
            	index  index.html index.htm;

	}
	location /ws {
           proxy_pass http://localhost:8090; 
           proxy_read_timeout 60s;
           proxy_http_version 1.1;
           proxy_set_header Upgrade $http_upgrade;
           proxy_set_header Connection 'Upgrade';
        } 
    }
}
~~~





#  二、搭建fastdfs分布式文件管理系统

第一个坑：**storage.conf中tracker_server必须用内网ip，不然storage启动不了**

第二个坑：**[emerg] unknown directive "ngx_fastdfs_module"**

~~~bash
./configure --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module --add-module=/usr/local/fastdfs/fastdfs-nginx-module/src
~~~

启动

~~~bash
/usr/bin/fdfs_trackerd /etc/fdfs/tracker.conf 
/usr/bin/fdfs_storaged /etc/fdfs/storage.conf
/usr/bin/fdfs_test /etc/fdfs/client.conf upload 要上传的文件
~~~

docker 启动

~~~shell

docker run -d --network=host --name=tracker -v /var/fdfs/tracker:/fastdfs/tracker season/fastdfs tracker

docker run -d --network=host --name=storage -v /var/fdfs/storage:/fastdfs/storage -v /var/fdfs/store_path:/fastdfs/store_path  -e TRACKER_SERVER=49.235.158.110:22122 -e GROUP_NAME=group1 season/fastdfs storage
~~~

~~~conf
location /group1/M00/{
                alias /var/fdfs/store_path/data/;
        }

~~~

