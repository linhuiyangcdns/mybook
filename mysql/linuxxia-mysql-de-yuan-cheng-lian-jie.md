如果Mysql是按上篇的方法进行安装和设置的话，那进行远程连接就会稍微简单一点。我就结合百度上的一些文章进行剖析。

**本地计算机ip：192.168.1.100  
远程计算机ip：192.168.1.244**



1. **远程连接上Linux系统，确保Linux系统已经安装上了MySQL数据库。登陆数据库。mysql -uroot -p（密码）。**

   ![](https://images2015.cnblogs.com/blog/1000464/201609/1000464-20160907112456223-345289006.jpg)

2. ** 创建用户用来远程连接**
   GRANT ALL PRIVILEGES ON \*.\* TO 'itoffice'@'%' IDENTIFIED BY 'itoffice' WITH GRANT OPTION;

   （第一个itoffice表示用户名，%表示所有的电脑都可以连接，也可以设置某个ip地址运行连接，第二个itoffice表示密码）。

   ![](https://images2015.cnblogs.com/blog/1000464/201609/1000464-20160907112518113-474329488.jpg)

3. ** 执行 flush privileges;命令立即生效**
   ![](https://images2015.cnblogs.com/blog/1000464/201609/1000464-20160907112527519-103594382.jpg)
4. **查询数据库的用户（看到如下内容表示创建新用户成功了）**
    SELECT DISTINCT CONCAT\('User: ''',user,'''@''',host,''';'\) AS query FROM mysql.user;

   ![](https://images2015.cnblogs.com/blog/1000464/201609/1000464-20160907112541066-201431566.jpg)

5. **使用exit命令退出MySQL**

   然后打开vim  /etc/mysql/my.cnf

   将bind-address    = 127.0.0.1

    设置成bind-address    = 0.0.0.0（设备地址）

   重新启动（命令如下）：

   /etc/init.d/mysql stop

   /etc/init.d/mysql start

   ![](https://images2015.cnblogs.com/blog/1000464/201609/1000464-20160907112800566-1357761182.jpg)

6. **查看端口号**
    show global variables like 'port';  

   ![](https://images2015.cnblogs.com/blog/1000464/201609/1000464-20160907112721254-1324340539.jpg)

7. **设置navicat连接。**
   ![](https://images2015.cnblogs.com/blog/1000464/201609/1000464-20160907112710941-365082354.jpg)
8. **点击连接测试看到如下内容表示成功。**
   ![](https://images2015.cnblogs.com/blog/1000464/201609/1000464-20160907112702863-347209546.jpg)

**注意问题：**

1）我经常在输入mysql -u root -p时，显示cannot connect to mysql server...这种错误，后来发现是没有打开mysql。所以在以上操作之前记得看看Mysql有没有打开：

```
1
 service mysql status --
查看有没有打开服务

2
 service mysql start --
打开服务

3
 service mysql stop --
停止服务

4
 service mysql restart --重启服务 
```

2）在进行上述连接后，我用navicat远程连接mysql，发现还是不可以，鼓捣了很久，才发现是因为防火墙的问题，防火墙把3306端口屏蔽了。所以要设置防火墙：

```
设置防火墙允许3306端口
vi 
/etc/sysconfig/
IPtables
添加
-A RH-Firewall-
1
-INPUT -m state --state NEW -m tcp -p tcp --dport 
3306
 -
j ACCEPT
（注意添加在
-A RH-Firewall-
1
-INPUT -j REJECT –reject-with icmp-host-
prohibited之前，否则可能导致规则不生效）
重启防火墙service iptables restart
```



**解决Mysql无法远程连接的问题**  
  
1、Mysql的端口是否正确  
通过**netstat -ntlp**查看端口占用情况，一般情况下端口是3306。在用工具连接MySQl是要用到端口。例如My Admin\My Query Browser\MySQl Front等。  
  
2、检查用户权限是否正确  
mysql库的user表里有两条记录：host分别为localhost和%\(为了安全，%可以换成你需要外部连接的IP\)。

