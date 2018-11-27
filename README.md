# nginx-proxy_pass-settings-for-dev
nginx proxy pass for dev

### nginx.conf
```javascript

worker_processes  1;

events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

     sendfile        on;

    keepalive_timeout  65;

    server {
        listen       8070;
        server_name  127.0.0.1;

	location / {
       proxy_pass   http://127.0.0.1:8081;
       proxy_redirect off;

       proxy_http_version 1.1;
       proxy_set_header Upgrade $http_upgrade;
       proxy_set_header Connection "upgrade";
    }
	
		location /api/ {
		proxy_pass http://localhost:5000/api/;
		proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $http_host;
        proxy_cache_bypass $http_upgrade;
		}
		
		location /swagger/ {
		proxy_pass http://localhost:5000/swagger/;
		proxy_http_version 1.1;
        proxy_set_header   Upgrade $http_upgrade;
        proxy_set_header   Connection keep-alive;
        proxy_set_header   Host $http_host;
        proxy_cache_bypass $http_upgrade;
	}
}
```
