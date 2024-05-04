
>[!info] 수동 설치 시 프로그램 파일은 공식 자료실에서 다운로드
>[[archive list]] 참조

>[!note]
>tomcat 연결중  포트포워딩 문제는 해당 명령어로 해결
>
>setsebool -P httpd_can_network_connect true

---
### 패키치 기준 설치
###### 1. 패키지 관리도구 업데이트

```js
[Centos7]
yum update -y

[Centos8 or Rockylinux]
dnf update -y

```

###### 2. nginx 설치

```js
[Centos7]
yum install nginx

[Centos8 or Rockylinux]
dnf install nginx
```

###### 3. Config 파일 확인
>기본경로는 /etc/nginx/conf

---

### 수동설치

1. 공식문서에서 rpm 다운로드 [[archive list]]

2. 

---

### nginx.conf 설정

~~~python
# For more information on configuration, see:
#   * Official English Documentation: http://nginx.org/en/docs/
#   * Official Russian Documentation: http://nginx.org/ru/docs/

user nginx;
worker_processes auto;
error_log /uploadData/service_dmz/logs/web/nginx/error.log;
pid /run/nginx.pid;

# Load dynamic modules. See /usr/share/doc/nginx/README.dynamic.
include /usr/share/nginx/modules/*.conf;

events {
    worker_connections 1024;
}

http {
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  /var/log/nginx/access.log  main;
    #access_log   /uploadData/service_dmz/logs/web/nginx/access.log  main;

    server_tokens off;
    access_log off;
    log_not_found off;
    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    types_hash_max_size 4096;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    client_body_buffer_size 10K;
    client_header_buffer_size 2k;
    client_max_body_size 1000M;
    large_client_header_buffers 50 1k;


    client_body_timeout 500s;
    send_timeout 500s;
    keepalive_timeout 1800s;
    proxy_read_timeout 86400s;
    proxy_send_timeout 86400s;


    include /etc/nginx/conf.d/*.conf;

}



~~~

---

### 서비스 conf 설정

#### 서브도메인 있을경우
~~~python
upstream univ {
    server 127.0.0.1:8080;
}

server {
    listen 80;
    server_name univ.hyundai-scholar.com;
    return 301 https://univ.hyundai-scholar.com$request_uri;
}


server {
    listen 443 ssl;
    server_name univ.hyundai-scholar.com;
    access_log /uploadData/service_dmz/logs/web/nginx/univ_access.log;

    ssl_certificate /etc/nginx/ssl/ngv_cert.pem;
    ssl_certificate_key /etc/nginx/ssl/ngv_key_nopass.pem;

    location / {
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_pass http://promotion;
        proxy_redirect off;
    }
}

~~~

#### 서브도메인 없을경우 (www.적용)
~~~python
upstream promotion {
    server 127.0.0.1:8080;
}

server {
    listen 80;
    server_name www.hyundai-scholar.com hyundai-scholar.com;
    return 301 https://www.hyundai-scholar.com$request_uri;
}

server {
    listen 443 ssl;
    server_name hyundai-scholar.com;

    ssl_certificate /etc/nginx/ssl/ngv_cert.pem;
    ssl_certificate_key /etc/nginx/ssl/ngv_key_nopass.pem;

    return 301 https://www.hyundai-scholar.com$request_uri;

}

server {
    listen 443 ssl;
    server_name www.hyundai-scholar.com;
    access_log /uploadData/service_dmz/logs/web/nginx/promotion_access.log;

    ssl_certificate /etc/nginx/ssl/ngv_cert.pem;
    ssl_certificate_key /etc/nginx/ssl/ngv_key_nopass.pem;

    location / {
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_pass http://promotion;
        proxy_redirect off;
    }
}

~~~




---

### log 관련 사항


#### 배치 필요 (logrotate사용)
>[!note]
>logrotate 는 linux OS에 기본적으로 내장되어있다. 
>혹시라도 내장되어있지않은 OS는 apache Web 서버 또는 다른 웹서버를 통해 끌어다가 사용 하면된다.
>왜냐하면 Web 서버들은 대게 배치성 library를 가지고 있다.
>

~~~python

*nginx*

#/var/log/nginx/*log {
/uploadData/service_dmz/logs/web/nginx/*log{
    copytruncate
    create 0664 nginx root
    daily
    rotate 180
    missingok
    notifempty
    compress
    delaycompress
    dateext
    dateformat -%Y%m%d-%s
    sharedscripts
    postrotate
        /bin/kill -USR1 `cat /run/nginx.pid 2>/dev/null` 2>/dev/null || true
    endscript
}


*tomcat*

/uploadData/service_dmz/logs/backend/tomcat/catalina/catalina.out {
        copytruncate
        daily
        rotate 180
        compress
        missingok
        notifempty
        dateext
        dateformat -%Y%m%d-%s
}



*적용후 확인*
sudo logrotate -f /etc/logrotate.d/nginx
sudo logrotate -f /etc/logrotate.d/tomcat

~~~



~~~python
해당파일 생성후 잘돌아가는지 돌려본다.
logrotate -f /etc/logrotate.d/nignx
logrotate -f /etc/logrotate.d/tomcat
~~~




###### 경로수정
~~~python




~~~