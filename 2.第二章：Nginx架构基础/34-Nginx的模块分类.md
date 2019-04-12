## 34-Nginx的模块分类
![img](https://raw.githubusercontent.com/fanpan26/nginx-study/master/nginx/nginx-34-20190412211822.png)

每个模块中的公共模块
```
event_core
ngx_http_core_module
ngx_stream_core_module
```
```
[root@localhost nginx-1.14.2]# cd src
[root@localhost src]# ll
total 20
drwxr-xr-x. 2 1001 1001 4096 Apr  2 14:29 core
drwxr-xr-x. 3 1001 1001 4096 Apr  2 14:29 event
drwxr-xr-x. 4 1001 1001 4096 Apr  2 14:29 http
drwxr-xr-x. 2 1001 1001 4096 Apr  2 14:29 mail
drwxr-xr-x. 2 1001 1001   72 Apr  2 14:29 misc
drwxr-xr-x. 3 1001 1001   17 Apr  2 14:29 os
drwxr-xr-x. 2 1001 1001 4096 Dec  4 09:52 stream

```
