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
