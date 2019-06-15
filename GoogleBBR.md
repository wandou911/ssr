# VPS服务器Google BBR一键安装脚本

### BBR介绍

BBR是Google的一套拥塞控制算法，用在VPS服务器上，可以有效减少拥堵丢包，大幅提高网络连接速度。

目前Linux类系统的最新内核，都已内置BBR。而我们购买VPS服务器时安装的系统，一般都不是最新的内核。怎么解决呢，在CentOS、Debian、Ubuntu等Linux系统上，可以通过升级最新内核的方式，获取BBR。􏰰􏰱􏰲􏰳􏰇􏰴􏰵􏰶􏰋􏰷􏰸 􏰺􏰻􏰦􏰣􏰧􏰨􏱼􏱽􏱾

BBR的安装也很简单，下面我们以Teddysun的一键安装包为例，介绍下BBR安装流程。

### BBR安装要求

系统要求：CentOS+、Debian7+、Ubuntu12+。

另外不支持OpenVZ虚拟的VPS服务器，这也是我们不建议购买OpenVZ服务器的原因之一。

  


### BBR一键安装
􏲓􏱔􏱉􏲔􏲕1.用Putty连接VPS服务器，输入以下命令运行：

```
wget --no-check-certificate https://github.com/teddysun/across/raw/master/bbr.sh && chmod +x bbr.sh && ./bbr.sh
```
小提示：如果运行命令时英文提示找不到wget的错误，那么先运行以下命令安装wget：

```
yum -y install wget
```

2.接下来BBR会自动开始安装，安装完成后会英文提示是否重启，输入y回车重启。

3.等待大概一分钟，系统重启成功后，重新用Putty连接VPS服务器，输入以下命令验证BBR是否安装成功：

```
sysctl net.ipv4.tcp_congestion_control
```

如果得到如下结果则代表BBR安装成功：

```
net.ipv4.tcp_congestion_control = bbr
```
VPS推荐：

[Vultr](https://www.vultr.com/?ref=7887711-4F) （vultr在2019年1月的最新活动，针对新用户，直接送50美元！vultr全球15个服务器位置可选，KVM框架，最低2.5美元/月。支持支付宝和paypal付款。）

<a href="https://www.vultr.com/?ref=7887711-4F"><img src="https://www.vultr.com/media/banner_2.png" width="468" height="60"></a>


#### 参考文献：

[VPS服务器Google BBR一键安装脚本](https://ssr.tools/199)

