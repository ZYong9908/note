# Nginx

## http

```nginx
http {
	server {
    listen 80;
    root /path/;
    index index.html;
    server_name www.web.com;
    access_log /var/log/nginx/www.web.com.access.log;
    error_log /var/log/nginx/www.web.com.error.log;
    location / {
        try_files $uri $uri/ =404;
        }
    }
}
```

## rtmp

```nginx
rtmp {
	server {
		listen 5002;
		chunk_size 4096;
		application live {
			live on;
			record off;
        }
    }
}
```

## 文件代理

```nginx
http {
    include /etc/nginx/server/;
	server {
	    listen 5001;
	    server_name localhost;
	    location /get_file/{
	        alias /file_path/;
        }
    }
}
```

## https

```nginx
 # HTTPS server
server {
    listen       443 ssl;
    server_name  localhost;
    ssl_certificate      cert.pem;
    ssl_certificate_key  cert.key;
    ssl_session_cache    shared:SSL:1m;
    ssl_session_timeout  5m;
    ssl_ciphers  HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers  on;
    location /path/ {
        root   html;
        index  index.html index.htm;
    }
}
```

## 跨域

```nginx
server {
    add_header Access-Control-Allow-Origin $http_origin;
    add_header Access-Control-Allow-Headers *;
    add_header Access-Control-Allow-Methods *;
}
```





