# Fuel 6.0 安装问题

-----------------------------

[TOC]


## VNC console 连接超时问题
该问题主要原因是proxy console需要进行dns 解析，但是我们在安装fuel master时设置dns的ip 为8.8.8.8，这个导致dns 解析超时，所以出现NOVNC connection timeout的问题。 目前解决办法两个：

-  设置正确的DNS IP
	-  在安装fuel master的时候，正确设置DNS IP
	-  如果安装时，没有设置正确，请修改fuel master 中/etc/fuel/astute.yaml 中DNS_UPSTREAM字段
-  在连接vnc时，避免DNS 解析
这个问题在nova upstream已经被解决，请参见下列步骤获取该patch

>  git clone https://github.com/openstack/nova.git
>  git format-patch  -1 c0f773a61

-----------------------------
# openstack dashboard 中文设置

**安装软件包**:

> apt-get install gettext

**编译2个语言包** ：

> cd /usr/share/openstack-dashboard/openstack_dashboard/locale/zh_CN/LC_MESSAGES
> msgfmt --statistics --verbose -o django.mo django.po

> cd /usr/share/pyshared/horizon/locale/zh_CN/LC_MESSAGES
> msgfmt --statistics --verbose -o django.mo django.po

**链接语言包** ：

> cd /usr/lib/python2.7/dist-packages/horizon/locale/zh_CN/LC_MESSAGES
> ln -s /usr/share/pyshared/horizon/locale/zh_CN/LC_MESSAGES/django.mo django.mo

**重启服务** ：

> sudo service apache2 restart


## 不能ping和ssh 登录虚拟机

openstack 中防火墙默认屏蔽icmp和tcp相应端口服务，所以为了必须使能icmp和ssh 服务

> nova secgroup-add-rule default icmp -1 -1 0.0.0.0/0
> nova secgroup-add-rule default tcp 22 22 0.0.0.0/0

## 使能虚拟机自动获取floating ip

Juno 版本openstack 已经支持自动获取floating ip

> Add auto_assign_floating_ip=true in /etc/nova/nova.conf
