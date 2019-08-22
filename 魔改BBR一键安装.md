# 南琴浪版暴力魔改BBR一键安装脚本

### BBR介绍

  BBR是Google出品的TCP拥塞控制算法，目前集成在最新的Linux内核中。国外VPS服务器上安装BBR后，可以明显提高连接速度，降低丢包。  BBR对SS/SSR有明显的加速作用，看Youbube视频时更为明显。另外如果在国外VPS服务器上架设网站，BBR也可以加速网站的加载速度。

魔改版BBR，则是在原版BBR基础上的修改版本，通过参数的修改，使加速算法更为激进，比原版BBR有更为明显的加速效果。
 
### BBR安装要求

系统要求：CentOS+、Debian7+、Ubuntu12+。

另外不支持OpenVZ虚拟的VPS服务器，这也是我们不建议购买OpenVZ服务器的原因之一。

  


### 魔改BBR一键安装
􏲓􏱔􏱉􏲔􏲕1.用Putty连接VPS服务器，输入以下命令运行：

```
wget --no-check-certificate https://raw.githubusercontent.com/tcp-nanqinlang/general/master/General/CentOS/bash/tcp_nanqinlang-1.3.2.sh
bash tcp_nanqinlang-1.3.2.sh
```
小提示：如果运行命令时英文提示找不到wget的错误，那么先运行以下命令安装wget：

```
yum -y install wget
```

2.出现下图提示时，输入数字1选择安装内核，然后回车：

![](https://ssr.tools/wp-content/uploads/2018-11-29_183122.jpg)

3.接下来的安装过程中，部分系统可能会有如下提示，提示删除旧的内核，是否取消。

这时按方向右键， <font color=red>选择No</font>后回车，确认删除。

```
sysctl net.ipv4.tcp_congestion_control
```
![](https://ssr.tools/wp-content/uploads/2018-11-29_185659.jpg)

4.出现如下提示后，输入reboot回车重启系统：

![](https://ssr.tools/wp-content/uploads/2018-11-29_185806.jpg)

5.系统重启完成后，重新Putty连接，输入以下命令重新运行脚本：

`bash tcp_nanqinlang-1.3.2.sh`

6.出现如下图提示后，<font color=red>输入2</font>选择安装并开启算法：

![](https://ssr.tools/wp-content/uploads/2018-11-29_183122.jpg)

7.稍等片刻，安装成功后的提示如下图：

![](https://ssr.tools/wp-content/uploads/2018-11-29_190039.jpg)

### 魔改BBR卸载

1.Putty连接VPS服务器，运行如下命令：

`bash tcp_nanqinlang-1.3.2.sh`

2.出现下图提示后，<font color=red>选择4</font>进行卸载：

![](https://ssr.tools/wp-content/uploads/2018-11-29_183122.jpg)

3.卸载完成后重启。

注意：此卸载仅卸载算法，并不卸载内核。

VPS推荐：

[Vultr](https://www.vultr.com/?ref=7887711-4F) （vultr在2019年1月的最新活动，针对新用户，直接送50美元！vultr全球15个服务器位置可选，KVM框架，最低2.5美元/月。支持支付宝和paypal付款。）

<a href="https://www.vultr.com/?ref=7887711-4F"><img src="https://www.vultr.com/media/banner_2.png" width="468" height="60"></a>


#### 参考文献：

[南琴浪版暴力魔改BBR一键安装脚本](https://ssr.tools/550)

