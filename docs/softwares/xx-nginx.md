## nginx 个人配置
### 主目录配置
```shell
server {
	listen 80;
	#listen       [::]:80 default_server;
	server_name  xx9527.cn www.xx9527.cn docs.xx9527.cn;
	return 307 https://$host$request_uri;
	# Load configuration files for the default server block.
	include /etc/nginx/default.d/*.conf;
}
```
### gzip配置
```sehll
gzip on;
gzip_vary on;
gzip_proxied any;
gzip_comp_level 6;
gzip_buffers 16 8k;
gzip_http_version 1.1;
gzip_types image/svg+xml text/plain text/html text/xml text/css text/javascript application/xml application/xhtml+xml application/rss+xml application/javascript application/x-javascript application/x-font-ttf application/vnd.ms-fontobject font/opentype font/ttf font/eot font/otf;
```
### ssl https 配置
```shell
server {
    listen       443 ssl;
    server_name  docs.xx9527.cn;
    root /xxx/xxx/xxx/;
    index index.html index.htm index.php;
    ssl_certificate xxx.pem; #ssl证书
    ssl_certificate_key xxx.key;#ssl证书
    ssl_session_cache shared:SSL:1m;
    ssl_session_timeout  10m;
    ssl_ciphers HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;
    location / {
    }
    error_page 404 /404.html;
        location = /40x.html {
    }

    error_page 500 502 503 504 /50x.html;
        location = /50x.html {
    }
}

```
