## 修改ssh端口的详细步骤（centos7）：


### 检查SElinux是否关闭

检查命令，输入看到是disable状态才行，要不需要先关闭掉，防止出现问题

![](https://www.daniao.org/wp-content/uploads/2017/11/cos-ssh-1.jpg)

1 查看当前selinux的状态。

`/usr/sbin/sestatus`

2 将SELINUX=enforcing 修改为 SELINUX=disabled 状态。

```
vi /etc/selinux/config

#SELINUX=enforcing 
SELINUX=disabled

```

3重启生效。reboot。

`reboot`


### 修改端口

1，控制SSH访问端口的文件为 /etc/ssh/sshd_config 。

    因此，编辑SSH配置文件sshd_config：

    #vi /etc/ssh/sshd_config



2，查找到 Port=22字段，将其前面的注释去掉：


    13  #Port 22        //将注释符#去掉 防止配置不好以后不能远程登录，等修改以后的端口能使用以后在注释掉

    14  #AddressFamily any

    15  #ListenAddress 0.0.0.0

    16  #ListenAddress ::



3，在这行下面再加同样的一行，端口号改为自己准备修改后的端口：


   	13  Port 22

    14  Port 2022        //新增一行，增加修改后的端口号

    15  #AddressFamily any

    16  #ListenAddress 0.0.0.0

    17  #ListenAddress ::

    
4，修改保存后，重启SSH服务：

    #/etc/init.d/sshd restart     //或者

    #service sshd restart 

5，这样，就可以通过2022远程访问linux主机了。

测试修改端口以后的ssh连接，如果成功则将port 22 重新注释掉

## 防火墙相关指令

1、开启关闭防火墙

```
systemctl start firewalld
systemctl stop firewalld
```

2、放行端口

```
firewall-cmd --permanent --zone=public --add-port=21212/tcp
firewall-cmd --reload
```


这里红字部分是我们设置的端口，需要放行。如果有出现"FirewallD is not running"问题可以参考"CentOS7出现的”Failed to start firewalld.service”问题以及端口添加记录"解决，没有启动导致的。

3、检查端口是否开启

```
firewall-cmd --permanent --query-port=21212/tcp

```
参考链接: [Linux centos 远程SSH默认22端口修改为其他端口](https://blog.51cto.com/chidongting/1761061)


参考链接：[CentOS7修改Linux服务器SSH默认端口的方法](https://www.daniao.org/2127.html)

