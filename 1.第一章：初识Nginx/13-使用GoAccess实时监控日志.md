## 13-使用GoAccess实时监控日志
安装 Goaccess 
```
$ wget https://tar.goaccess.io/goaccess-1.3.tar.gz
$ tar -xzvf goaccess-1.3.tar.gz
$ cd goaccess-1.3/
$ ./configure --enable-utf8 --enable-geoip=legacy
$ make
# make install
```
运行 Goaccess
```
[root@localhost logs]# goaccess access.log -o ../html/report.html --log-format=COMBINED --real-time-html &
[2] 27303
WebSocket server ready to accept new client connections
```
修改 conf ,增加 report.html 访问配置
```
  #goaccess log   
  location /report.html {
     alias /usr/local/nginx/html/report.html;
  }
```
访问 http://server-addr/report.html
![img](https://raw.githubusercontent.com/fanpan26/nginx-study/master/nginx/nginx-20190406105135.png)