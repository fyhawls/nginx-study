## 38-用好共享内存的工具：Slab管理器
![img](https://raw.githubusercontent.com/fanpan26/nginx-study/master/nginx/nginx-38-20190416203320.png)
![img](https://raw.githubusercontent.com/fanpan26/nginx-study/master/nginx/nginx-38-20190416203625.png)

下载 Tengine
```
[root@localhost download]# wget http://tengine.taobao.org/download/tengine-2.3.0.tar.gz
--2019-04-16 17:35:53--  http://tengine.taobao.org/download/tengine-2.3.0.tar.gz
Resolving tengine.taobao.org (tengine.taobao.org)... 140.205.172.18
Connecting to tengine.taobao.org (tengine.taobao.org)|140.205.172.18|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 2389514 (2.3M) [application/octet-stream]
Saving to: ‘tengine-2.3.0.tar.gz’

100%[=================================================================================================================================================>] 2,389,514   1.44MB/s   in 1.6s   

2019-04-16 17:35:56 (1.44 MB/s) - ‘tengine-2.3.0.tar.gz’ saved [2389514/2389514]
```
查看ngx_slab_stat模块
```
[root@localhost tengine-2.3.0]# cd modules
[root@localhost modules]# ll
total 24
drwxrwxr-x. 2 root root   48 Mar 25 00:16 ngx_backtrace_module
drwxrwxr-x. 3 root root 4096 Mar 25 00:16 ngx_debug_pool
drwxrwxr-x. 3 root root   95 Mar 25 00:16 ngx_debug_timer
drwxrwxr-x. 2 root root   50 Mar 25 00:16 ngx_http_concat_module
drwxrwxr-x. 2 root root   57 Mar 25 00:16 ngx_http_footer_filter_module
drwxrwxr-x. 9 root root 4096 Mar 25 00:16 ngx_http_lua_module
drwxrwxr-x. 3 root root   65 Mar 25 00:16 ngx_http_proxy_connect_module
drwxrwxr-x. 2 root root   76 Mar 25 00:16 ngx_http_reqstat_module
drwxrwxr-x. 2 root root   49 Mar 25 00:16 ngx_http_slice_module
drwxrwxr-x. 2 root root   52 Mar 25 00:16 ngx_http_sysguard_module
drwxrwxr-x. 2 root root 4096 Mar 25 00:16 ngx_http_tfs_module
drwxrwxr-x. 2 root root   55 Mar 25 00:16 ngx_http_trim_filter_module
drwxrwxr-x. 2 root root   97 Mar 25 00:16 ngx_http_upstream_check_module
drwxrwxr-x. 2 root root   68 Mar 25 00:16 ngx_http_upstream_consistent_hash_module
drwxrwxr-x. 2 root root   60 Mar 25 00:16 ngx_http_upstream_dynamic_module
drwxrwxr-x. 2 root root 4096 Mar 25 00:16 ngx_http_upstream_dyups_module
drwxrwxr-x. 2 root root 4096 Mar 25 00:16 ngx_http_upstream_keepalive_module
drwxrwxr-x. 2 root root   67 Mar 25 00:16 ngx_http_upstream_session_sticky_module
drwxrwxr-x. 2 root root   54 Mar 25 00:16 ngx_http_user_agent_module
drwxrwxr-x. 3 root root 4096 Mar 25 00:16 ngx_slab_stat
[root@localhost modules]# clear
[root@localhost modules]# ll
total 24
drwxrwxr-x. 2 root root   48 Mar 25 00:16 ngx_backtrace_module
drwxrwxr-x. 3 root root 4096 Mar 25 00:16 ngx_debug_pool
drwxrwxr-x. 3 root root   95 Mar 25 00:16 ngx_debug_timer
drwxrwxr-x. 2 root root   50 Mar 25 00:16 ngx_http_concat_module
drwxrwxr-x. 2 root root   57 Mar 25 00:16 ngx_http_footer_filter_module
drwxrwxr-x. 9 root root 4096 Mar 25 00:16 ngx_http_lua_module
drwxrwxr-x. 3 root root   65 Mar 25 00:16 ngx_http_proxy_connect_module
drwxrwxr-x. 2 root root   76 Mar 25 00:16 ngx_http_reqstat_module
drwxrwxr-x. 2 root root   49 Mar 25 00:16 ngx_http_slice_module
drwxrwxr-x. 2 root root   52 Mar 25 00:16 ngx_http_sysguard_module
drwxrwxr-x. 2 root root 4096 Mar 25 00:16 ngx_http_tfs_module
drwxrwxr-x. 2 root root   55 Mar 25 00:16 ngx_http_trim_filter_module
drwxrwxr-x. 2 root root   97 Mar 25 00:16 ngx_http_upstream_check_module
drwxrwxr-x. 2 root root   68 Mar 25 00:16 ngx_http_upstream_consistent_hash_module
drwxrwxr-x. 2 root root   60 Mar 25 00:16 ngx_http_upstream_dynamic_module
drwxrwxr-x. 2 root root 4096 Mar 25 00:16 ngx_http_upstream_dyups_module
drwxrwxr-x. 2 root root 4096 Mar 25 00:16 ngx_http_upstream_keepalive_module
drwxrwxr-x. 2 root root   67 Mar 25 00:16 ngx_http_upstream_session_sticky_module
drwxrwxr-x. 2 root root   54 Mar 25 00:16 ngx_http_user_agent_module
drwxrwxr-x. 3 root root 4096 Mar 25 00:16 ngx_slab_stat
[root@localhost modules]# cd ngx_slab_stat
[root@localhost ngx_slab_stat]# ll
total 44
-rw-rw-r--. 1 root root   204 Mar 25 00:16 config
-rw-rw-r--. 1 root root  6627 Mar 25 00:16 ngx_http_slab_stat_module.c
-rw-rw-r--. 1 root root  3465 Mar 25 00:16 README.cn
-rw-rw-r--. 1 root root  3501 Mar 25 00:16 README.md
-rw-rw-r--. 1 root root 21430 Mar 25 00:16 slab_stat.patch
drwxrwxr-x. 2 root root    19 Mar 25 00:16 t
```
进入到openresty，重新编译
```
[root@localhost local]# cd openresty-1.13.6.2
[root@localhost openresty-1.13.6.2]# ll
total 112
drwxrwxr-x. 44 panzi panzi  4096 Apr  5 21:59 build
drwxrwxr-x. 43 panzi panzi  4096 May 14  2018 bundle
-rwxrwxr-x.  1 panzi panzi 48140 May 14  2018 configure
-rw-rw-r--.  1 panzi panzi 22924 May 14  2018 COPYRIGHT
-rw-r--r--.  1 root  root   5807 Apr  5 22:00 Makefile
drwxrwxr-x.  2 panzi panzi  4096 May 14  2018 patches
-rw-rw-r--.  1 panzi panzi  4689 May 14  2018 README.markdown
-rw-rw-r--.  1 panzi panzi  8972 May 14  2018 README-windows.txt
drwxrwxr-x.  2 panzi panzi    50 May 14  2018 util
```
添加模块
```
[root@localhost openresty-1.13.6.2]# ./configure --add-module=../tengine-2.3.0/modules/ngx_slab_stat/
```
安装
```
[root@localhost openresty-1.13.6.2]# make
[root@localhost openresty-1.13.6.2]# make install
```
修改配置文件
```
lua_shared_dict dogs 10m;

    server {
        listen       80;
        server_name  localhost;
        location /set {
           content_by_lua_block{
             local dogs = ngx.shared.dogs;
             dogs:set("Jim",8);
             ngx.say("STORED");
           }
        }
        location /get{
            content_by_lua_block {
             local dogs = ngx.shared.dogs;
             ngx.say(dogs:get("Jim"));
            }
        }
        location = /slab_stat {
          slab_stat;
        }

```
重新启动，查看结果
```
[root@localhost nginx]# ./sbin/nginx 
[root@localhost nginx]# curl http://localhost/set
STORED
[root@localhost nginx]# curl http://localhost/get
8
[root@localhost nginx]# curl http://slab_stat
^[[A^H^H^H^C
[root@localhost nginx]# curl http://localhost/slab_stat
* shared memory: dogs
total:       10240(KB) free:       10168(KB) size:           4(KB)
pages:       10168(KB) start:00007F02A212A000 end:00007F02A2B1A000
slot:           8(Bytes) total:           0 used:           0 reqs:           0 fails:           0
slot:          16(Bytes) total:           0 used:           0 reqs:           0 fails:           0
slot:          32(Bytes) total:         127 used:           1 reqs:           1 fails:           0
slot:          64(Bytes) total:           0 used:           0 reqs:           0 fails:           0
slot:         128(Bytes) total:          32 used:           2 reqs:           2 fails:           0
slot:         256(Bytes) total:           0 used:           0 reqs:           0 fails:           0
slot:         512(Bytes) total:           0 used:           0 reqs:           0 fails:           0
slot:        1024(Bytes) total:           0 used:           0 reqs:           0 fails:           0
slot:        2048(Bytes) total:           0 used:           0 reqs:           0 fails:           0

```
