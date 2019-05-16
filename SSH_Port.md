## 修改ssh端口的详细步骤（centos7）：

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


参考链接: [Linux centos 远程SSH默认22端口修改为其他端口](https://blog.51cto.com/chidongting/1761061)