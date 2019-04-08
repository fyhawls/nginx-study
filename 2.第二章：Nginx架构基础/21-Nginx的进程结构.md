## 21-Nginx的进程结构
![img](https://raw.githubusercontent.com/fanpan26/nginx-study/master/nginx/nginx-21-20190408200020.png)

一个Master进程，多个Worker进程，Master进程负责管理Worker进程。Worker进程负责处理请求。