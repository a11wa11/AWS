# AWS

#!/bin/bash
yum update -y
localectl set-locale LANG=ja_JP.utf8
timedatectl set-timezone Asia/Tokyo
yum install -y tree
yum install -y epel-release
yum install -y xrdp tigervnc-server
yum groupinstall -y "GNOME Desktop"
cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bk
cp /etc/xrdp/xrdp.ini /etc/xrdp/xrdp.ini.bk
sed -i s/enabled=1/enabled=0/g /etc/yum.repos.d/epel.repo
sed -i s/"PasswordAuthentication no"/"PasswordAuthentication yes"/ /etc/ssh/sshd_config
sed -i s/max_bpp=32/max_bpp=16/ /etc/xrdp/xrdp.ini
systemctl start firewalld
firewall-cmd --permanent --zone=public --add-port=3389/tcp
firewall-cmd --reload
systemctl set-default multi-user.target
systemctl stop packagekit
systemctl mask packagekit
systemctl start xrdp
systemctl enable xrdp
