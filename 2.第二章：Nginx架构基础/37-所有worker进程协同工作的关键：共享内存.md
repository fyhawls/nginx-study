## 37-所有worker进程协同工作的关键：共享内存
![img](https://raw.githubusercontent.com/fanpan26/nginx-study/master/nginx/nginx-37-20190416202452.png)
![img](https://raw.githubusercontent.com/fanpan26/nginx-study/master/nginx/nginx-37-20190416202750.png)
![img](https://raw.githubusercontent.com/fanpan26/nginx-study/master/nginx/nginx-37-20190416203029.png)

共享内存，进程之间通过自旋锁访问共享内存
限速，流控等需要在共享内存中做
红黑树，快速的插入和删除