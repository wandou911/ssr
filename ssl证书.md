有时候我们为了测试会用一个临时的域名来测试，比如test.toodyao.com，测试完成后也许会置之不理，但是在90天更新证书的时候总会有这个域名的失败提示，很不爽，而且lnmp下只提供add，没有del指令，遂手动删之。

1. 找到Let’s Encrypt的目录

对于lnmp安装的用户，目录在 /etc/letsencrypt

2. 删除

需要删除 live ， archive ， renewal 三个目录下你想删除的域名文件/目录，都是以域名命名

另外，如果没有更新证书的话，90天后会自动过期，过期前会有邮件提醒

参考：https://zhaodi.me/remove-domains-from-lets-encrypt-ssl-tls-certificate/

————

2018-3-1 更新

需要删除nginx下相应域名的vhost，否则在重启的时候，会提示找不到该域名的SSL文件而启动失败，如下：

```

Starting nginx... nginx: [emerg] BIO_new_file("/etc/letsencrypt/live/test.toodyao.com/fullchain.pem") failed (SSL: error:02001002:system library:fopen:No such file or directory:fopen('/etc/letsencrypt/live/test.toodyao.com/fullchain.pem','r') error:2006D080:BIO routines:BIO_new_file:no such file)
 failed
 
 ```
如果使用的是lnmp，直接键入 lnmp vhost del ，然后键入你想删除的域名即可

nginx关闭的时候SSR竟然也用不了，奇怪

3 使用certbot命令删除证书

进入 /etc/letsencrypt/renewal 目录

```
[root@vultr ~]# cd /etc/letsencrypt/renewal

[root@vultr live]# ls
README  www.xiaokeli.me

[root@vultr renewal]# certbot delete --cert-name www.xiaokeli.me
Saving debug log to /var/log/letsencrypt/letsencrypt.log

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Deleted all files relating to certificate www.xiaokeli.me
```

4 certbot更新证书 

```
[root@vultr certbot]# service nginx stop
Stoping nginx...  done
[root@vultr certbot]# certbot renew
Saving debug log to /var/log/letsencrypt/letsencrypt.log

[root@vultr certbot]# service nginx start

```
