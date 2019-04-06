## 18-使用免费证书实现一个HTTPS站点
安装 certbot
```
[root@localhost ~]# yum install python2-certbot-nginx
```
执行命令
```
certbot --nginx --nginx-server-root=/usr/local/openresty/nginx/conf/ -d real-domain
```
执行certbot --nginx命令生成秘钥时, 报错如下：
Saving debug log to /var/log/letsencrypt/letsencrypt.log
The nginx plugin is not working; there may be problems with your existing configuration.
The error was: NoInstallationError()
查看debug日志内容如下：
DEBUG:certbot.plugins.disco:No installation (PluginEntryPoint#nginx):
Traceback (most recent call last):
File "/usr/lib/python2.7/site-packages/certbot/plugins/disco.py", line 126, in prepare
self._initialized.prepare()
File "/usr/lib/python2.7/site-packages/certbot_nginx/configurator.py", line 131, in prepare
raise errors.NoInstallationError
经过google，看了几篇帖子发现：
certbot生成证书时，需要读取默认nginx，需要将openresty/nginx/sbin/nginx加入到PATH里 or ln -s /usr/local/openresty/nginx/sbin/nginx /usr/local/sbin/nginx
然后就能才正确安装证书了
