## 关机 重启

1.reboot 普陀重启

2.shutdown -r now 立刻重启（root用户）

3.shutdown -r 10 \(10分钟后重启\)

4.shutdown -r 20.35 \(20.35重启）

halt 立刻关机 poweroff

shutdown -c取消重启

## chchmod权限

chmod -R lamport:users \*

-rw------- \(600\) -- 只有属主有读写权限。

-rw-r--r-- \(644\) -- 只有属主有读写权限；而属组用户和其他用户只有读权限。

-rwx------ \(700\) -- 只有属主有读、写、执行权限。

-rwxr-xr-x \(755\) -- 属主有读、写、执行权限；而属组用户和其他用户只有读、执行权限。

-rwx--x--x \(711\) -- 属主有读、写、执行权限；而属组用户和其他用户只有执行权限。

-rw-rw-rw- \(666\) -- 所有用户都有文件读、写权限。这种做法不可取。

-rwxrwxrwx \(777\) -- 所有用户都有读、写、执行权限。更不可取的做法。

## $PATH环境变量配置

\# **vim /etc/profile**

**在文档最后，添加:**

export PATH="/opt/STM/STLinux-2.3/devkit/sh4/bin:$PATH"

保存，退出，然后运行：

\#**source /etc/profile**

不报错则成功。

## 查看文件内容

&lt;!--  
br  
{  
mso-data-placement:same-cell;  
}  
table  
{  
mso-displayed-decimal-separator:"\.";  
mso-displayed-thousand-separator:"\, ";  
}  
tr  
{  
mso-height-source:auto;  
mso-ruby-visibility:none;  
}  
td  
{  
border:.5pt solid windowtext;  
}  
.NormalTable{cellspacing:0;cellpadding:10;border-collapse:collapse;mso-table-layout-alt:fixed;border:none; mso-border-alt:solid windowtext .75pt;mso-padding-alt:0cm 5.4pt 0cm 5.4pt; mso-border-insideh:.75pt solid windowtext;mso-border-insidev:.75pt solid windowtext}  
.fontstyle0  
{  
	font-family:微软雅黑;  
	font-size:12pt;  
	font-style:normal;  
	font-weight:normal;  
	color:rgb\(0,0,0\);  
}  
.fontstyle1  
{  
	font-size:12pt;  
	font-style:normal;  
	font-weight:normal;  
	color:rgb\(0,0,0\);  
}  
.fontstyle2  
{  
	font-family:Symbol;  
	font-size:10pt;  
	font-style:normal;  
	font-weight:normal;  
	color:rgb\(0,0,0\);  
}  
--&gt;  


| cat 由第一行开始显示档案内容 tac 从最后一行开始显示，可以看出 tac 是 cat 癿倒着写！ nl 显示癿时候，顺道输出行号！ more 一页一页癿显示档案内容 less 不 more 类似，但是比 more 更好癿是，他可以往前翻页！ head 叧看头几行 tail 叧看尾巳几行 od 以二迚制癿方式读取档案内容！ |
| :--- |


  


  




