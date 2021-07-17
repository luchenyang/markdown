### Top命令

- PID : 进程id
- USER : 进程所有者的用户名
-  PR : 进程优先级
- NI : nice值。负值表示高优先级，正值表示低优先级 
- VIRT : 进程使用的虚拟内存总量，单位kb
- SHR : 共享内存大小
-  %CPU : 上次更新到现在的CPU时间占用百分比 
- %MEM : 进程使用的物理内存百分比
- TIME+ : 进程使用的CPU时间总计，单位1/100秒 
- COMMAND : 命令名称、命令行



打印匹配行的前后5行

- grep -5 ‘something’ file

打印匹配行的前后5行

- grep -C 5 ‘something’ file

打印匹配行的后5行

- grep -A 5 ‘something’ file

打印匹配行的前5行

- grep -B 5 ‘something’ file





systemctl restart mysqld

rabbitmq-server -detached 后台启动服务

