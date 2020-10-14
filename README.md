# single
singlemodepasswd

centos7 设置grub密码及单用户登录实例

centos7与centos6在设置grub密码的操作步骤上有很大的差别，特此记录供以后查用

grub加密的目的: 防止不法分子利用单用户模式修改root密码

给grub加密可以采用明文或者加密的密文两种，建议使用加密的密文方式，两者操作步骤上相差不多，本文以加密的密文为例
一.设置grub加密

1.使用grub2-mkpasswd-pbkdf2命令创建密文（一定的保存记住自己设置的密码）

2.在/etc/grub.d/00_header 文件末尾，添加以下内容  (root 为单用户登录使用的用户名，第三行root后面为上一步加密后得到的密文，注意root和密文之间是空格隔开的不是换行符)

cat <<EOF
set superusers='root'
password_pbkdf2 root grub.pbkdf2.sha512.10000.B157F42E96462AB239C03000F113D32EB18FD48073F1FC7D8F87A8F3B3F89F662424ECCAB901F3A812A997E547FD520F3E99D0E080F4FE8B05E019757E34F75B.29C83F87B4B6C086FC9A81E046CC3623CC5CF2F82128EDC3A0364894E429D4993B28563F82D71BF346188108CBD4341FC4A71B90E543581646B4E7EAE920C54A
E0F

3.重新编译生成grub.cfg文件

grub2-mkconfig  -o  /boot/grub2/grub.cfg

设置完成。

 
二.重启使用单用户登录测试

1.reboot进入gurb界面

2.按e

3.这个时候需要我们输入我们设置的进入gurb的用户名和密码进入grub（看到这个界面说明我们已经设置grub加密生效了） ,输入正确后会进入到以下的界面

4.编辑修改两处：ro改为rw, 以及在该行的最后面添加init=/bin/sh

 

5.ctrl+x 启动单用户模式进入系统

6.修改root密码

7.如果selinux是开启着的则需要执行以下命令更新系统信息,否则重启之后密码未生效
1
	
touch /.autorelabel

8.重启系统

exec /sbin/init

 

使用修改后的root密码登录成功。

REF:https://www.cnblogs.com/heaven-xi/p/9990558.html
