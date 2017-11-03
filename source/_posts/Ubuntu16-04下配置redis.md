---
title: Ubuntu16.04下配置redis
date: 2017-11-03 15:44:43
tags: 
    - Ubuntu
    - linux
    - redis
category: linux
---
# 下载redis
{% codeblock %}
wget http://download.redis.io/releases/redis-4.0.2.tar.gz
{% endcodeblock %}
# 解压redis
{% codeblock %}
tar -zxvf redis-4.0.2.tar.gz
{% endcodeblock %}
# 进入redis目录，并编译redis
{% codeblock %}
cd redis-4.0.2.tar.gz
make //编译redis，需要提前安装gcc
{% endcodeblock %}
# 安装redis
{% codeblock %}
sudo make PREFIX=/usr/local/redis install //安装到/usr/local/redis目录下
{% endcodeblock %}
# 注册服务
{% codeblock %}
# 1.创建redis配置目录
sudo mkdir /etc/redis
# 2.从解压包中拷贝配置文件
sudo cp ~/下载/redis-4.0.2/redis.conf /etc/redis/6379.conf
# 3.修改配置文件
sudo vim /etc/redis/6379.conf
    # 打开后台运行选项
    daemonize yes
    # 设置日志文件路径
    logfile /var/log/redis/redis.log
    # 设置持久化文件存放路径
    dir /var/lib/redis 
    # 注意：上面的两个文件夹没有创建，需要启动服务前创建
    sudo mkdir /var/log/redis
    sudo mkdir /var/lib/redis
# 4.从解压包中拷贝配置服务脚本
sudo cp ~/下载/redis-4.0.2/utils/redis_init_script /etc/init.d/redis
# 5.修改服务脚本
EXEC=/usr/local/redis/bin/redis-server
CLIEXEC=/usr/local/redis/bin/redis-cli
# 6.设置服务脚本有执行权限
sudo chmod +x /etc/init.d/redis
# 7.注册服务
sudo update-rc.d redis defaults
{% endcodeblock %}
# 遇到问题
{% codeblock %}
# script 'redis' missing LSB tags and overrides问题的解决办法
sudo vim /etc/init.d/redis
在文件头部`#!/bin/sh`下面添加以下代码：
### BEGIN INIT INFO
# Provides: OSSEC HIDS
# Required-Start: $network $remote_fs $syslog $time
# Required-Stop:
# Default-Start: 2 3 4 5
# Default-Stop: 0 1 6
# Short-Description: OSSEC HIDS
### END INIT INFO
{% endcodeblock %}
# 服务命令
{% codeblock %}
开启redis： service redis start
停止redis： service redis stop
重启redis： service redis restart
查看服务状态：service redis status
{% endcodeblock %}

