## 10-命令行演示
下载静态资源站点`layui fly` 社区网站代码
```
git clone https://github.com/layui/fly.git
```
将文件上传到linux服务器上。/usr/local/nginx/app/fly
```
[root@localhost nginx]# cd app
[root@localhost app]# ll
total 4
drwxr-xr-x. 6 root root 4096 Apr  2 18:58 fly
[root@localhost app]# cd fly
[root@localhost fly]# ll
total 16
-rw-r--r--. 1 root root  441 Apr  2 18:58 CHANGELOG.md
drwxr-xr-x. 7 root root   99 Apr  2 18:58 html
drwxr-xr-x. 2 root root   22 Apr  2 18:58 json
-rw-r--r--. 1 root root 1061 Apr  2 18:58 LICENSE
-rw-r--r--. 1 root root 1335 Apr  2 18:58 README.md
drwxr-xr-x. 6 root root   52 Apr  2 18:58 res
drwxr-xr-x. 7 root root 4096 Apr  2 18:58 views
```
修改配置文件
```
[root@localhost conf]# vim nginx.conf
 server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        access_log  logs/host.access.log  main;

        location / {
            alias  app/fly/;
            index  index.html index.htm;
        }
```
重新加载配置文件 
```
[root@localhost nginx]# ./sbin/nginx -s reload
```
网站正常运行
![img](https://raw.githubusercontent.com/fanpan26/nginx-study/master/nginx/nginx-20190405184210.png)
添加gzip压缩
```
    gzip  on;
    gzip_comp_level 2;
    gzip_types text/plain application/x-javascript text/css text/javascript image/png image/jpeg image/jpg;
```
使用 autoindex 
```
autoindex on;
```
![img](https://raw.githubusercontent.com/fanpan26/nginx-study/master/nginx/nginx-20190405185104.png)
记录日志
```
log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        access_log  logs/host.access.log  main;
```
查看日志
```
[root@localhost logs]# vim host.access.log

192.168.187.1 - - [02/Apr/2019:18:16:17 -0400] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.110 Safari/537.36" "-"
192.168.187.1 - - [02/Apr/2019:18:16:19 -0400] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.110 Safari/537.36" "-"
192.168.187.1 - - [02/Apr/2019:18:16:19 -0400] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.110 Safari/537.36" "-"
192.168.187.1 - - [02/Apr/2019:18:16:20 -0400] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.110 Safari/537.36" "-"
192.168.187.1 - - [02/Apr/2019:18:16:20 -0400] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.110 Safari/537.36" "-"
192.168.187.1 - - [02/Apr/2019:18:16:20 -0400] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.110 Safari/537.36" "-"
192.168.187.1 - - [02/Apr/2019:18:16:20 -0400] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.110 Safari/537.36" "-"
192.168.187.1 - - [02/Apr/2019:18:16:20 -0400] "GET / HTTP/1.1" 304 0 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.110 Safari/537.36" "-"
192.168.187.1 - - [02/Apr/2019:19:05:10 -0400] "GET / HTTP/1.1" 403 571 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.110 Safari/537.36" "-"
192.168.187.1 - - [02/Apr/2019:19:05:15 -0400] "GET /index.html HTTP/1.1" 404 571 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.110 Safari/537.36" "-"
192.168.187.1 - - [02/Apr/2019:19:05:23 -0400] "GET /html/index.html HTTP/1.1" 200 35478 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.110 Safari/537.36" "-"
192.168.187.1 - - [02/Apr/2019:19:05:23 -0400] "GET /res/layui/css/layui.css HTTP/1.1" 200 52949 "http://192.168.187.129/html/index.html" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.110 Safari/537.36" "-"
192.168.187.1 - - [02/Apr/2019:19:05:23 -0400] "GET /res/css/global.css HTTP/1.1" 200 49201 "http://192.168.187.129/html/index.html" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.110 Safari/537.36" "-"
192.168.187.1 - - [02/Apr/2019:19:05:23 -0400] "GET /res/layui/layui.js HTTP/1.1" 200 6144 "http://192.168.187.129/html/index.html" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.110 Safari/537.36" "-"
192.168.187.1 - - [02/Apr/2019:19:05:23 -0400] "GET /res/images/logo.png HTTP/1.1" 200 2781 "http://192.168.187.129/html/index.html" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.110 Safari/537.36" "-"
192.168.187.1 - - [02/Apr/2019:19:05:23 -0400] "GET /res/layui/font/iconfont.woff?v=220 HTTP/1.1" 200 24684 "http://192.168.187.129/res/layui/css/layui.css" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.110 Safari/537.36" "-"
192.168.187.1 - - [02/Apr/2019:19:05:23 -0400] "GET /res/mods/index.js?v=3.0.0 HTTP/1.1" 200 19536 "http://192.168.187.129/html/index.html" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.110 Safari/537.36" "-"
192.168.187.1 - - [02/Apr/2019:19:05:23 -0400] "GET /res/layui/lay/modules/layer.js?v=3.0.0 HTTP/1.1" 200 22088 "http://192.168.187.129/html/index.html" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.110 Safari/537.36" "-"
192.168.187.1 - - [02/Apr/2019:19:05:23 -0400] "GET /res/layui/css/modules/layer/default/layer.css?v=3.1.0 HTTP/1.1" 200 14424 "http://192.168.187.129/html/index.html" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.110 Safari/537.36" "-"
192.168.187.1 - - [02/Apr/2019:19:05:23 -0400] "GET /res/layui/lay/modules/jquery.js?v=3.0.0 HTTP/1.1" 200 97647 "http://192.168.187.129/html/index.html" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.110 Safari/537.36" "-"

```