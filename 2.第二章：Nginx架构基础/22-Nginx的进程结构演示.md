## 21-Nginx的进程结构
查看nginx进程
```
[root@localhost nginx]# ps -ef | grep nginx
root      23253      1  0 Apr05 ?        00:00:00 nginx: master process ./sbin/nginx
nobody    27768  23253  0 Apr05 ?        00:00:00 nginx: worker process
root      64772  64602  0 11:45 pts/1    00:00:00 grep --color=auto nginx
```
执行 ./sbin/nginx -s reload 会 kill 一个work进程，重新启动一个worker进程
```
[root@localhost nginx]# ./sbin/nginx -s reload
[root@localhost nginx]# ps -ef | grep nginx
root      23253      1  0 Apr05 ?        00:00:00 nginx: master process ./sbin/nginx
nobody    64824  23253  0 11:47 ?        00:00:00 nginx: worker process
root      64826  64602  0 11:47 pts/1    00:00:00 grep --color=auto nginx
```
22768->64824

向Master进程发送 HUP 命令，也会得到同样的效果。
```
[root@localhost nginx]# kill -SIGHUP 23253
[root@localhost nginx]# ps -ef | grep nginx
root      23253      1  0 Apr05 ?        00:00:00 nginx: master process ./sbin/nginx
nobody    64960  23253  0 11:50 ?        00:00:00 nginx: worker process
root      64964  64602  0 11:50 pts/1    00:00:00 grep --color=auto nginx
```
64824->64960

向Worker进程发送 TERM 命令
```
[root@localhost nginx]# kill -SIGTERM 64960
[root@localhost nginx]# ps -ef | grep nginx
root      23253      1  0 Apr05 ?        00:00:00 nginx: master process ./sbin/nginx
nobody    65052  23253  0 11:51 ?        00:00:00 nginx: worker process
root      65054  64602  0 11:51 pts/1    00:00:00 grep --color=auto nginx
```
64960->65052