# win10常用网络命令

**1、 ping 测试网络连能性，使用ICMP 协议。**

ping IP|域名

TTL 生存时间：2n-TTL>=0

2n-50>=0

26-50=14

2n-127>=0

27-127=1

**2、 ipconig 查看IP配置情况**

ipconfig /ALL 显示所有信息

ipconfig /displaydns  显示本地域名缓存。

Ipconfig /flushdns  清除本地域名缓存

**3、arp 显示或者修改arp缓存表**

Arp –a

Arp -s iP  MAC

**4、tracert 路由跟踪令**

Tracert ip|域名

**5、netstat  查看端口使用情况**

 Netstat –o 

**6、route  查看本地计算机路由配置情况。**

  Route print

**7、pathping 带路由的ping**

 Pathping  www.baidu.com

**8、net**

  Net start 程序

  Net stop 程序

**9、FTP**

 ftp 10.20.133.10

**10、nslookup 查看DNS**