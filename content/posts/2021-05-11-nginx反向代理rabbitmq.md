---
title: Nginx反向代理RabbitMQ
author: Kubehan
type: post
date: 2021-05-11T08:06:43+08:00
url: /3203.html
post_style:
  - sidebar
cao_vip_rate:
  - 1
views:
  - 1049
categories:
  - Linux运维

---
1、安装nginx步骤省略  
2、添加下面的location配置到nginx.conf

<pre><code class="language-bash"> location /rabbitmq/api/ {
            rewrite ^ $request_uri;
            rewrite ^/rabbitmq/api/(.*) /api/$1 break;
            return 400;
            proxy_pass http://192.168.1.100:15672$uri;
            proxy_buffering                    off;
            proxy_set_header Host              $http_host;
            proxy_set_header X-Real-IP         $remote_addr;
            proxy_set_header X-Forwarded-For   $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
        location /rabbitmq/ {
            port_in_redirect on;
            proxy_redirect off;
            proxy_pass http://192.168.1.100:15672/;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header User-Agent $http_user_agent;
            proxy_set_header Host $http_host;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
            rewrite ^/rabbitmq/(.*)$ /$1 break;
        }</code></pre>

重启nginx即可

<pre><code class="language-bash">nginx -s reload</code></pre>