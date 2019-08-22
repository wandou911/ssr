# sprov-ui，支持多协议多用户的v2ray管理面板


##查看面板支持的功能

### 支持的功能

https 访问面板

系统运行状态监控

多协议、多用户管理

禁用、启用单个账号

支持设置监听的 IP（多 IP 服务器下）

流量统计（支持所有协议）

### 支持的 v2ray 协议

vmess（v2ray 特色）

shadowsocks（经典 ss）

mtproto（Telegram 专用）

dokodemo-door（端口转发）

socks（socks 4、socks 4a、socks 5）

http（http 代理）

### 支持的 vmess 传输配置

tcp
kcp + 伪装
ws + 伪装 + tls
http/2 + 伪装 + tls

## 一、sprov-ui 安装&升级

推荐内存在 256MB 及以上的 vps 安装，在低内存的 vps 下可能运行不佳，你可以试试设置虚拟内存来缓解内存压力。

支持的系统，最好是纯净版的系统，在精简版的系统中可能会安装失败：

CentOS 7（推荐）

Ubuntu 16

Ubuntu 18

Debian 8

Debian 9

### 一键管理脚本

>以下四组命令任君选择，效果都是一样的，如果下载脚本有报错则尝试另一组。

>请务必使用 root 用户运行脚本，否则会出问题：sudo su root，或者在命令前加上 sudo

```
wget -O /usr/bin/sprov-ui -N --no-check-certificate https://blog.sprov.xyz/sprov-ui.sh && chmod +x /usr/bin/sprov-ui && sprov-ui
```

```
wget -O /usr/bin/sprov-ui -N --no-check-certificate https://github.com/sprov065/sprov-ui/raw/master/sprov-ui.sh && chmod +x /usr/bin/sprov-ui && sprov-ui
```

```
yum install wget -y
apt-get install wget -y
wget -O sprov-ui https://blog.sprov.xyz/sprov-ui.sh
chmod +x sprov-ui
mv sprov-ui /usr/bin/ -f
sprov-ui
```

```
yum install wget -y
apt-get install wget -y
wget -O sprov-ui https://github.com/sprov065/sprov-ui/raw/master/sprov-ui.sh
chmod +x sprov-ui
mv sprov-ui /usr/bin/ -f
sprov-ui
```

运行管理脚本会出现如下界面，这就不用多说了吧，输入 1，并回车即可安装面板。

![一键管理脚本](https://blog.sprov.xyz/wp-content/uploads/2019/05/image-2.png)


安装面板可能需要几分钟时间，该脚本会安装 Java 环境、v2ray（如果已安装则会强制升级到最新版）、以及关闭防火墙，安装过程中会让你输入面板监听端口、登录用户名和密码，推荐自己定义，不使用默认的。


![面板安装完成](https://blog.sprov.xyz/wp-content/uploads/2019/05/image-3.png)

### 查看手动安装或升级面板方法

### 安装谷歌bbr

参考这篇文章：[谷歌bbr – tcp加速工具](https://blog.sprov.xyz/2019/02/04/bbr-tcp-faster/)

## 二、sprov-ui 使用

sprov-ui 使用 systemd 来管理，以下是常用的命令，使用这些命令时，可能不会输出任何信息，也可能会输出一些信息，都是正常的。如果输出了错误信息，比如包含了 error 等字样，才代表出错了，需要排查错误。

```
systemctl start sprov-ui      # 启动sprov-ui
systemctl restart sprov-ui    # 重启sprov-ui
systemctl stop sprov-ui       # 关闭sprov-ui
systemctl status sprov-ui     # 查看sprov-ui运行状态
systemctl enable sprov-ui     # sprov-ui开机启动
systemctl disable sprov-ui    # 取消sprov-ui开机启动
```

>以上都是老的管理办法，现在也可以使用，或者也可以使用一键管理脚本，一键管理脚本在文章前面有说明。

启动 sprov-ui 后，在浏览器地址栏中输入你的服务器 IP 加冒号端口号，切记是英文的冒号，不是中文的冒号，访问 sprov-ui 面板网页。


使用你设置的用户名和密码登录 sprov-ui 面板。


![登录](https://blog.sprov.xyz/wp-content/uploads/2019/05/image-1.png)

登录后可以看到你的服务器的基本状态以及 v2ray 的运行状况。

此页面显示的是系统的状态，比如【已运行】表示的是服务器已运行的时间，而不是 v2ray 运行的时间。【网络】和【流量】显示的是整个服务器外网网卡的网络速度和流量，而不是 v2ray 的，如果你发现面板统计了内网的流量，请告知我你的系统版本和所有网卡名称，我将作出修正。


![系统运行状态](https://blog.sprov.xyz/wp-content/uploads/2019/04/blog.sprov.xyz-snipaste-2019-04-06-21-52-06.png)

账号列表可以看到你的 v2ray 账号，在这里你可以选择添加多个账号，或者删除、修改账号，添加、修改、删除账号之后记得点击【重启】来使配置生效。

【重置】是重置流量的功能，每个账号可单独重置流量，最上面的【重置】按钮可重置所有账号的流量。


![账号列表](https://blog.sprov.xyz/wp-content/uploads/2019/04/blog.sprov.xyz-snipaste-2019-04-06-21-52-07.png)

## 三、客户端使用

可以在[这篇文章]()https://blog.sprov.xyz/2019/02/04/v2ray-simple-use/的第五章看起，包括Windows、macOS、安卓、iOS的教程。

## 四、常见问题

sprov-ui 启动失败：Address already in use

这个问题是因为面板的监听端口被占用了，换个端口即可。

sprov-ui 启动失败：port out of range:xxxx

面板监听的端口超出正常范围，正常范围是1-65535，换个端口即可。

sprov-ui 启动失败：Invalid keystore format

证书有问题，需要 jks 格式的证书，文章下面有配置教程。

sprov-ui 启动失败：Keystore was tampered with, or password was incorrect

jks 证书密码错误，如果忘记密码了可以重新生成一个。

vmess 协议的账号连不上，其它的账号都连得上，端口也是通的

这是因为你的服务器时间和本地时间相差过大，vmess 协议要求服务器的 UTC 时间和本地 UTC 时间相差不超过 90 秒，服务器与本地的时区不一样没关系，但是分钟数要相同，请自行修改服务器时间。

所有账号都连不上，或者刚刚添加/修改的账号连不上

添加、删除、修改账号之后都需要重启 v2ray 才会生效新的配置，点击网页上的【重启】按钮即可，不是【重启面板】。还有确保你的端口是通的，防火墙都放行了。

开启 v2ray api 失败：xxxx

这个错误的原因一般就是你的 v2ray 配置文件格式过老了，v2ray 的 v4.1 版本开始启用了新的配置文件格式，本面板只支持 v4.1 版本之后的配置文件。

通用解决方法：

先备份好你的 v2ray 节点信息
删除 /etc/v2ray/config.json 文件
重新使用此命令安装 v2ray：bash <(curl -L -s https://install.direct/go.sh) -f
重启面板
访问网页出现：Bad response. The server or forwarder response doesn’t look like HTTP.

开启 https 后不能使用 http:// 访问，请使用 https:// 访问，且必须使用域名访问，不能使用 ip。

## 五、高级操作

### 使用域名

首先你需要一个域名，并将域名解析到你 vps 的 IP，直接使用域名加端口号登录面板即可，无需其它配置。

### sprov-ui 面板配置文件

面板配置文件在 /etc/sprov-ui/ 文件夹下，包含两个文件，一个是 sprov-ui.conf，一个是 v2ray-extra-config.json。

### sprov-ui.conf

```
port=80                       # 面板监听端口
username=sprov                # 用户名
password=blog.sprov.xyz       # 密码
keystoreFile=                 # jks 证书文件路径，v3.0.0+
keystorePass=                 # jks 证书密码，v3.0.0+
maxWrongPassCount=5           # 密码最大错误次数，达到此次数则封禁 IP，默认为 5，v3.0.0+
loginTitle=xxxx               # 登录页标题，可自定义，留空则没有标题，v3.1.0+
loginFooter=xxxx              # 登录页页脚，可自定义，支持 html 标签，留空则没有页脚，v3.1.0+
```

此文件配置了 sprov-ui 的端口、用户名、密码等等，可以自行修改，修改后需要重启面板生效，留空则使用默认配置。配置错误会导致面板启动失败，例如：port=111111（非法端口号）。

### v2ray-extra-config.json

```
{
    "disabled-inbounds": [],
    "inbounds": []
}
```

此文件为 v2ray 配置文件的扩展，为一个 json 文件，包含两个属性：inbounds、disabled-inbounds。

inbounds 为一个数组，包含若干个 inbound，主要记录流量数据，每个 inbound 的格式如下：

```
{
    "tag": "",      // tag 标识，不能为空
    "downlink": 0,  // 下行流量，单位 Byte
    "uplink: 0      // 上行流量，单位 Byte
}
```
disabled-inbounds 为一个数组，包含若干个 inbound，记录被禁用的 inbound，每个 inbound 都是一个完整的 v2ray inbound，并且还包含流量数据。

### 启用面板 https 访问

sprov-ui v3.0.0 及以上版本支持

>开启 https 后不能再使用 http:// 访问面板，请使用 https:// 访问面板，且必须使用域名访问，不能使用 ip

首先我们申请的证书常见的是 .crt / .pem 等格式，密钥常见的格式是 .key，这些证书不能直接配置在面板里，需要将证书和密钥转换成 .jks 格式的证书，以下是转换教程。

将 crt / pem 证书转换为 jks 格式的证书

在转换 jks 证书后，上传 jks 证书至你的服务器，并在面板配置文件里添加如下配置，并重启面板：

```
keystoreFile=/path/to/xxx.com.jks        # jks 证书文件绝对路径
keystorePass=yourpassword                # jks 证书密码，如果没有设置密码则留空
```

不会上传文件到服务器？看这篇文章：MobaXterm – 一个强大的全能终端

### 面板服务器迁移

面板的服务器迁移很简单，首先需要备份面板配置文件和 v2ray 配置文件，分别是：/etc/sprov-ui/ 文件夹下所有文件，/etc/v2ray/config.json，如何备份请自行解决。

然后在新服务器上重新安装面板，之前备份的文件覆盖掉现有的，最后启动或重启面板即可。

## 六、总结

以上就是我花了几天来开发的 v2ray 面板，名字叫 sprov-ui，如果你在使用中遇到了问题，或者你有更多的建议，推荐加入 Telegram 群组讨论，也可以在下方评论，或者在 Github 提 issue。

参考链接：

[sprov-ui，支持多协议多用户的v2ray管理面板](https://blog.sprov.xyz/2019/02/09/sprov-ui/)