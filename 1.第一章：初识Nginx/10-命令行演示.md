## 10-命令行演示
命令行
* 格式：nginx-s reload
* 帮助：-? -h
* 使用指定的配置文件：-c
* 指定配置指令：-g
* 指定运行目录：-p
* 发送信号： -s stop/quit/reload/reopen
* stop 立即停止服务，quit 优雅的停止服务 reload 重载配置文件 reopen 重新开始记录日志文件
* 测试配置文件是否有语法错误：-t -T
* 打印nginx版本信息，编译信息等：-v -V
