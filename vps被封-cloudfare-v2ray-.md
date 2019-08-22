
前言：IP被封的检测和解决思路已经写成系列文章，最新信息请查看[《搬瓦工vps的IP被封(0) 前言及日志目录》](https://eveaz.com/1093.html)。

以下正文：

1、先行检测：
参考[《搬瓦工vps的IP被封(1) 如何检测及更换IP》](https://eveaz.com/1076.html)一文方法。

2、情况描述：IP被全面封锁，具体表现如下：
2.1、ICMP和TCP快速检测；
![](http://ww1.sinaimg.cn/large/006tNc79gy1g46huv80v4j30jg0adjs2.jpg)

2.2、全球ping检测；

![](https://eveaz.com/wp-content/uploads/2019060402.jpg)

3、准备工作
3.1、注册域名或使用现有域名；
3.1.1、本博客使用的域名是在NameSilo注册的，输入本站域名“eveaz.com”可以直减1美刀；阿里云、其它渠道或者免费域名请自行搜索。

3.2、vps安装Cenots7系统（建议重装至全新，以免出现错误）；
3.2.1、博主使用的是[”BandwagonHOST(搬瓦工)“](https://bandwagonhost.com/aff.php?aff=19935)的vps，6.25%优惠码 BWH26FXH3HIQ 。目前最具有性价比的vps是（CN2 GT线路洛杉矶机房，1核1G，20G SSD，流量1T/月）；

3.3、确保SSH连接vps成功；
3.3.1、Xshell隧道代理SSH，参考[《搬瓦工vps的IP被封(2) Xshell如何设置代理SSH》）](https://eveaz.com/1077.html)；
3.3.2、vps控制面板后台SSH（不建议，例如搬瓦工KiviVM面板下Basic shell下运行文中所用v2ray脚本报错；Interactive Shell不支持中文）；

3.4、注册Cloudflare账号；

3.5、想一劳永逸？那我建议你用 [（Just My Socks）搬瓦工自建机场](https://justmysocks.net/members/aff.php?aff=2765) ，域名和vps都不需要注册和购买了；机场IP被封自动换IP，走搬瓦工CN2 GIA线路，月付2.8刀起，5.2%优惠码 JMS9272283 。

3.6、以上如果需要代购，联系邮箱cocofive#foxmail.com。

4、具体实施步骤如下
4.1、在Cloudflare添加域名，并确保域名能在Cloudflare正常使用；
4.1.1、登陆到Cloudflare，进入域名管理页面，点击“Add a Site”按钮；
![](http://ww3.sinaimg.cn/large/006tNc79ly1g46hxgad9bj30jg07yjrs.jpg)

4.1.2、填写主域名，点击“Add a Site”按钮；

![](http://ww1.sinaimg.cn/large/006tNc79ly1g46hxtor73j30jg0c2aap.jpg)

4.1.3、Cloudflare查询DNS，无视掉，点击“NEXT”；

![](http://ww1.sinaimg.cn/large/006tNc79ly1g46hy0iph3j30jg0d6myd.jpg)

4.1.4、勾选 “FREE”，点击 “Confirm plan”；

![](http://ww1.sinaimg.cn/large/006tNc79ly1g46hy61yqdj30jg0fg0uc.jpg)

4.1.5、弹出“Confirm plan”窗口，点击“Confirm”；

![](http://ww4.sinaimg.cn/large/006tNc79ly1g46hyeqkcgj30jg07n3zc.jpg)


4.1.6、这个时候Cloudflare会提取之前的解析记录，如果不需要修改，直接无视；添加二级域名A记录解析到被封的IP，点击 “Add Record”；

![](http://ww2.sinaimg.cn/large/006tNc79ly1g46hymrci1j30jg04ejrq.jpg)

4.1.7、新增解析之后，是这样的；

![](http://ww3.sinaimg.cn/large/006tNc79ly1g46hysqtxxj30jg03vwes.jpg)


4.1.8、点击“Status”栏那朵小黄云，让小黄云变灰色，即（DNS only）；点击“Continue”；Cloudflare的操作暂时结束，不要关闭网页；

![](http://ww3.sinaimg.cn/large/006tNc79ly1g46hz4f1xtj30jg04u3yv.jpg)

4.2、域名DNS服务器修改；
4.2.1、新标签页打开已经注册的域名解析管理后台，修改DNS服务器为Cloudflare的DNS服务器（Cloudflare的DNS服务器信息参考4.2.2中截图，请根据4.1.8操作后获取的DNS信息修改，也就是那个提醒过不要关闭的网页）；这里不做演示，根据注册商后台自行修改；
4.2.2、修改完毕之后，回到4.1.8步骤未关闭的Cloudflare网页，点击 “Continue”，进入到Cloudflare域名管理页面；
![](http://ww4.sinaimg.cn/large/006tNc79ly1g46hzcie6hj30jg0cbjsc.jpg)

4.2.3、当在Cloudflare域名管理页面看到如下图时，说明域名服务器更改还未生效，等待生效就行；一般几分钟就可以生效的；

![](http://ww2.sinaimg.cn/large/006tNc79ly1g46hzm2qaoj30jg0gzgnh.jpg)

4.2.4、当在Cloudflare域名管理页面看到如下图时，说明域名服务器已经更换成功；

![](http://ww3.sinaimg.cn/large/006tNc79ly1g46hzr6osjj30jg06lwfd.jpg)


4.3、在vps上安装V2Ray；

4.3.1、root用户SSH登陆到vps；

请先确认Firewalld是否开启，如果是开启的，在Firewalld中开放v2ray端口和80、443端口；

如果对Firewalld不熟悉，请直接关闭Firewalld；


防火墙相关指令

1、开启关闭防火墙 查看防火墙状态
```
systemctl start firewalld
systemctl stop firewalld
systemctl status firewalld
```
2、放行端口
```
firewall-cmd --permanent --zone=public --add-port=21212/tcp
firewall-cmd --reload

```


3、检查端口是否开启
```
firewall-cmd --permanent --query-port=21212/tcp
```
运行v2ray安装脚本

`bash <(curl -s -L https://git.io/v2ray.sh)`

```
## 如果提示 curl: command not found ，那是因为vps没装 Curl
## centos 系统安装 Curl 方法: yum update -y && yum install curl -y
## 安装好 curl 之后就能安装脚本了
```

4.3.2、运行脚本之后进入脚本界面，选“1”安装；

![](http://ww2.sinaimg.cn/large/006tNc79ly1g46hzzbyp5j30jg05jq36.jpg)

4.3.3、选择V2Ray传输协议，选“4. WebSocket+TLS”；

![](http://ww1.sinaimg.cn/large/006tNc79ly1g46i32ua7ij30jg0ewdhf.jpg)

4.3.4、输入V2Ray端口，默认就行（指定端口的话，不能是80或者443端口），直接回车；

![](http://ww3.sinaimg.cn/large/006tNc79ly1g46i399kiyj30jg06hq3c.jpg)


4.3.5、输入刚才在Cloudflare新增的二级域名，回车；确认域名是正确的，按“Y”，继续；

![](http://ww2.sinaimg.cn/large/006tNc79ly1g46i3d56fmj30jg065wez.jpg)

4.3.5.1、如果域名不对或者解析未生效（一般域名解析需要等待一段时间来生效），会提示检测域名解析错误，直接退出脚本，如下图；

![](http://ww1.sinaimg.cn/large/006tNc79ly1g46i3hqnr9j30jg04o3yu.jpg)


4.3.6、正确解析域名之后，继续运行脚本；域名解析检测正确，安装Caddy自动配置TLS，按“Y”继续；

![](http://ww4.sinaimg.cn/large/006tNc79ly1g46i3myvswj30jg0b5q3t.jpg)


4.3.7、是否开启网站伪装和路径分流，默认否；是否开启广告拦截，默认否；是否配置Shadowsocks，默认否；准备安装，确认配置正确，按回车继续；

![](http://ww2.sinaimg.cn/large/006tNc79ly1g46i3rfm4qj30jg0h475l.jpg)


4.3.8、接下来是安装过程，安装完成之后会显示V2Ray的配置信息；提示输入 v2ray url 可生成 vmess URL 链接 / 输入 v2ray qr 可生成二维码链接，无视掉；

![](http://ww4.sinaimg.cn/large/006tNc79ly1g46i3wa5avj30jg09hgmf.jpg)


4.3.9、 输入v2ray status 查看运行状态，确认V2Ray和Caddy都在运行；

![](http://ww3.sinaimg.cn/large/006tNc79ly1g46i404pfdj30jg025dfs.jpg)

4.4、Cloudfare设置SSL
4.4.1、进入Cloudflare域名管理页面，打开“Crypto”选项卡，确认“SSL”状态为“Full”；确认“Universal SSL Status”状态为“Active Certificate”，如果没有，请等待，是在申请证书，24小时之内可以申请下来（也许几分钟就好了，反正我是这样的）；
![](http://ww3.sinaimg.cn/large/006tNc79ly1g46i47fox6j30jg0bajt0.jpg)

4.4.2、打开DNS选项卡，将之前 4.1.8 操作点成灰色的小云点亮，重新变成小黄云，即 DNS and HTTP proxy(CDN)，需要等待一段时间才能生效；

![](http://ww3.sinaimg.cn/large/006tNc79ly1g46i4dmbnzj30jg0f3mz4.jpg)

4.4.3、点亮之后是这样的；

![](http://ww4.sinaimg.cn/large/006tNc79ly1g46i4icmc5j30jg03vwes.jpg)


4.5、配置V2Ray客户端，SSH输入v2ray info可以查看V2Ray的配置信息，根据客户端设置就行了；
（提示: 输入 v2ray 可以进行脚本管理；输入 v2ray url 可生成 vmess url 链接；输入 v2ray qr 可生成二维码链接）

![](http://ww4.sinaimg.cn/large/006tNc79ly1g46i4p9nlrj30jg0c0jsg.jpg)


4.5.1、移动端小火箭直接通过vmess链接导入；
4.5.2、win系统参考《搬瓦工vps的IP被封(3) V2Ray+mKCP部署及V2RayN客户端配置使用教程》
4.5.3、刷了梅林固件的路由器在科学上网插件里面新增v2ray节点；

![](http://ww4.sinaimg.cn/large/006tNc79ly1g46i4p9nlrj30jg0c0jsg.jpg)

5、步骤结束。

2019年06月11日补充更新

6、其它事项：
6.1、如果有遇到任何问题，可以在下方留言；请先排查前面已经完成的步骤有没有问题，然后再详细描述在哪个步骤出现问题，报错信息是什么；

6.2、留言的时候请务必填写正确邮箱用于接收留言回复，本站开启了严格的留言审核，如果留言提交未生效，请勿多次提交；

6.3、如果步骤一步步按文章来，是不会出现问题的，出现各样问题只能是操作错误或者配置错误。

6.4、文章06月04日发布，截至2019年6月11日20:42分，已经通过邮件或者即时通讯工具帮助27位网友解决了问题（不一定包括文章留言中的网友）；遇到的问题千奇百怪，本人精力有限，不一定能够即时回复留言或者邮件。

6.5、所以，不会配置，或者出现各种问题导致各种报错、无法科学上网却找不到原因的，可以联系联系邮箱cocofive#foxmail.com。邮件内容需要提供问题描述及报错信息，按需求可能需要提供域名注册商、Cloudflare的注册账号信息及SSH的root权限信息。当然，如果你觉得这些信息很重要不方便提供就算了。

2019年06月13日补充更新

6.6、校园网用户，如果通过ss或者V2RayN客户端代理Xshell，SSH连接不上vps或者卡着不动，请将本地网络切换至其它非校园网网络，如手机热点；

6.7、重装至Centos7之后，安装脚本，Caddy无法正常运行，请先确认Firewalld是否开启，如果是开启的，在Firewalld中开放v2ray端口和80、443端口；如果对Firewalld不熟悉，请直接关闭Firewalld；

查看Firewalld状态：`systemctl status firewalld` 或者 firewall-cmd –state

停止：`systemctl stop firewalld`

禁用：`systemctl disable firewalld`

6.8、IOS的小火箭用户，无法导入vmess链接或者手动填写无WebSocket选项，请更新至最新版；

2019年06月14日补充更新

6.9、如果配置正确且是独立网络环境连接的情况下，A设备能正常上网，但是B设备却不能上网，请检查B设备的时间是否是同步网络时间（客户端与服务端的时间误差不能超过90秒钟，与时区无关）。


参考链接：

[搬瓦工vps的IP被封(4) Cloudflare+V2Ray+WebSocket+TLS](https://eveaz.com/1094.html)
