---
title: 搭建nginx-https
date: 2019-02-25 12:24:07
tags: [nginx]
---
## nginx https
defalut.conf
```

    upstream www.aaa.com.38240.r1.up {

        server 127.0.0.1:8443  weight=1  max_fails=1 fail_timeout=10s;
    }

 server {

        listen                        38240;
        server_name                   registry.wise2c.com;

        access_log                    /var/log/nginx/host.access.log  main;
        error_log                     /var/log/nginx/host_error.log;


        # for support ssl

        ssl                           on;
        ssl_certificate               /etc/nginx/registry.wise2c.com.crt;
        ssl_certificate_key           /etc/nginx/registry.wise2c.com.key;
        ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
        ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
        ssl_prefer_server_ciphers on;


       proxy_headers_hash_max_size     51200;
       proxy_headers_hash_bucket_size  6400;
       proxy_set_header                X-Forwarded-For  $remote_addr;
       proxy_set_header                Host             $http_host;
       proxy_set_header                X-Real-IP        $remote_addr;
       proxy_set_header                X-Forwarded-For  $proxy_add_x_forwarded_for;

       add_header Upgrade "TLS/1.2, HTTP/1.1";
       add_header Connection "Upgrade";
       add_header PPP  https://$host:$server_port$uri/;
       error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }


        location / {
                proxy_pass                http://www.aaa.com.38240.r1.up/;
                proxy_connect_timeout     2s;
                proxy_redirect http:// $scheme://;
      }

    }
```

```

https http

端口(88)相同  域名不相同  

访问http://www.xx.com:88     400  
访问https://www.xxx.com:88     200

端口相同  域名相同  只有https的存在

问题
nginx log

2019/02/25 08:10:46 [warn] 1#1: conflicting server name "registry.wise2c.com" on 0.0.0.0:38240, ignored
nginx: [warn] conflicting server name "registry.wise2c.com" on 0.0.0.0:38240, ignored

端口不相同  域名相同 200


```

## Nginx环境下http和https可同时访问方法
```

server {
listen 80 default backlog=2048;
listen 443 ssl;
server_name x.x.x;
root /var/www/html;
ssl_certificate /usr/local/Tengine/sslcrt/domain.crt;
ssl_certificate_key /usr/local/Tengine/sslcrt/domain.Key;

 location / {
                proxy_pass                http://www.aaa.com.38240.r1.up/;
                proxy_connect_timeout     2s;
                proxy_redirect http:// $scheme://;
      }
}
```




```

https://www.ixsystems.com/community/threads/how-to-owncloud-using-nginx-php-fpm-and-mysql.17786/page-78#post-395092
过添加

client_body_in_file_only clean 找到解决方案; 
client_body_buffer_size 32K; #set 

max upload size 
client_max_body_size 4000M; 


       add_header X-Xss-Protection“1; mode = block”总是;
	   add_header X-Content-Type-Options“nosniff”总是;
	   add_header Strict-Transport-Security“max-age = 2592000; includeSubdomains”总是;
	   add_header X-Frame-Options“SAMEORIGIN”总是;
	   proxy_hide_header X-Powered-By;
	   add_header'Referrer-Policy''no-referrer';
	   add_header Content-Security-Policy“frame-ancestors mydomain.eu;”;
```


dragonfly docker proxy   branch fix-md2

