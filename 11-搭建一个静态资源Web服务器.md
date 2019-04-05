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