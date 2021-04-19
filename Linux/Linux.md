**查看已经开放的端口**

firewall-cmd --list-ports


**2.开启指定端口**

firewall-cmd --zone=public --add-port=2181/tcp --permanent

**3.关闭指定端口**

firewall-cmd --zone=public --remove-port=2181/tcp --permanent


**4.重启防火墙**

firewall-cmd --reload