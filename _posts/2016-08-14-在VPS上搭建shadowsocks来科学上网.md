---
layout: post
title: 在VPS上搭建shadowsocks来科学上网
category: shadowsocks
tags: [shadowsocks]
---

之前一直在使用免费的ss，但是访问外网的速度实在是不可恭维，索性决定自己搭建VPS来翻墙。本来准备在[搬瓦工](https://bandwagonhost.com/)购买，不过最低配置也要$19.9/Y。  

后来意外发现 GitHub Education 有面向学生的福利，一旦成功申请到Github Student pack，就可以获得$50，加上充值的$5和小伙伴给的邀请链接$10,此时账户一共有$65，对于最低配置的VPS $5一个月，可以使用一年啊！！直到毕业之前都一直有Promo Code赠送，那就远远不止免费一年了。

推荐有能力，爱折腾，有科学上网需要的小伙伴食用此教程。
这是得到Student Developer Pack时Git发来的邮件的部分内容：

> Spread the word: we love giving educational discounts to students, teachers, administrators, and researchers! Please send them to:  
        https://education.github.com 

真是暖心啊~~~业界良心！！！
具体过程参考以下内容：

## 获取Student Developer Pack

如果你不是学生，请跳过第一步。如果你是学生的话，首先你要 [注册GitGub帐号](https://github.com/)，然后前往 [GitHub Education](https://education.github.com/)获取Student Developer Pack，点击 I am a student 填写学生验证信息， Verify academic status 一栏选择 I don't have a school-issued email （第一次我使用学校的 edu 邮箱提交申请当天就被拒绝了，貌似国内edu邮箱没有公信力），然后上传你的学生证照片或者能证明你是学生即可。剩下的就是漫长的等待了，不过这点等待还是很值得的（本人等了接近2天终于收到确认信件）~~~

![这里写图片描述](http://img.blog.csdn.net/20160816164348813)

![这里写图片描述](http://img.blog.csdn.net/20160816164637677)

##  Digital Ocean

### 注册DO帐号

首先注册Digital Ocean账号。可以点击我的[邀请链接](https://m.do.co/c/6d3c33c4b39e)注册，激活后就会收到$10的奖励，但是仅仅这样还是无法在Digital Ocean上创建虚拟主机。你需要绑定信用卡或者使用PayPal添加银行卡，作为国内用户建议不要使用国内信用卡，有说会不成功，导致账号直接废了。通过paypal充值的方法验证账户的好处是如果有纠纷可以通过paypal纠纷申请退款，并且paypal资金安全性要好于信用卡绑定。PayPal需要支付$5才能完成注册流程。我是在paypal账号上绑定了一张银联的卡来付款的，$5按汇率大概￥33多一点吧。

![这里写图片描述](http://img.blog.csdn.net/20160816161413413)

* 具体方法参考[Digitalocean VPS注册和使用详细中文教程](http://www.hi8688.com/695.html) 的前四个步骤就好。

* 如果你已经获得了了第一步的Student Developer Pack，那么可以在注册成功后在Settings->Billing下找到Promo Code，输入你在Student Developer Pack获得的学生优惠码，价值50刀，这对于屌丝学生来说可是笔不小的数目。

### 创建VPS

然后你就可以创建你的VPS了。操作系统的话，我选择的是ubuntu的，看个人喜好选择吧。搭建SS服务器选择 $5/mon 的最低端的配置就够了，如有你有建站或者其他需求的话另行选择。经过测试在国内使用Singapore的机房是最优的选择，而其他的的机房延迟都很高。最后点击Create创建。

Tips:贴一个测速地址，请根据测速结果选择最合适的机房。 [Digitalocean服务器测速-新加坡机房](http://speedtest-sgp1.digitalocean.com/)

## ShadowSocks

### SS介绍

[ShadowSocks](https://github.com/shadowsocks)是科学上网的利器，在 Github 上接近1.4W的 Star，使用的人极多、影响极深。而且它对各个平台的支持也非常好，目前我在Windows/Android平台上都完成了科学上网环境的搭建。

### 配置shadowsocks服务端

创建成功后，在你的 droplets 控制面板上的服务器名后点击下拉 more ，再点击 accessv console 进入远程终端的连接。  ，
![这里写图片描述](http://img.blog.csdn.net/20160816163553145)

*  Tips:记下你的服务器IP地址，配置SS客户端会用到。

![这里写图片描述](http://img.blog.csdn.net/20160816163154280)

初次进入需要输入原始的账号密码，账号是root，密码会在注册邮箱中找到（创建完 droplet 后发送至邮箱，注意有可能在邮件垃圾箱里），然后输入Username 和 Password回车确认( Tips:输入密码时不会有暗文 "********" 显示，尽管输入然后回车确定就好。而且初始密码有点长，需要有点耐心。^-^)，系统会让你再次输入原始的密码（current）UNIX password, 然后Enter new UNIX password 输入新密码，然后 Retype 确认密码。至此，VPS远程登陆成功。

### 在VPS上安装shadowsocks

输入以下命令安装shadowsocks：

```
# apt-get update
# apt-get install python-pip
// 询问是否Continue，输入y确认
# pip install shadowsocks
```

### 启动的SS服务

```
sudo ssserver -p 443 -k yourpassword -m aes-256-cfb --user nobody -d start
```

###  SS客户端下载

前往GitHub托管的[ShadowSocks](https://github.com/shadowsocks)项目下载相应客户端。  

这里仅贴出windows版本：[下载地址](https://github.com/shadowsocks/shadowsocks-windows/releases/download/3.4.3/Shadowsocks-3.4.3.zip)

###  客户端配置

服务器->添加->填入以下项:

```
服务器地址：你的服务器IP
服务器端口：443
密码： "yourpassword"
加密： "aes-256-cfb"
```

配置完成后在托盘图标里勾选启动代理，就可以访问外网，获取你所需要的资源了。


贴张图表示下 ---> YouTube 1080P 毫无压力

![这里写图片描述](http://img.blog.csdn.net/20160819235904373)


> 如有疑问请留言或者给我来信。 Email:  waynechu@waynechu.cn   

文章来自:  [拓扑部落（topblog.top）>>](http://www.topblog.top) [在VPS上搭建shadowsocks来科学上网](http://www.topblog.top/?p=60)
