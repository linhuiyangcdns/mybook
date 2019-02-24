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

## 使用--prefix参数指定nginx安装的目录,make、make install安装



