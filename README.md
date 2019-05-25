# netspeeder-onekeyinstall
ALL COPY FROM IMEIJI
CentOS一键安装

wget --no-check-certificate https://raw.githubusercontent.com/wjk199511140034/netspeeder-onekeyinstall/master/net_speeder_lazyinstall.sh
sh net_speeder_lazyinstall.sh

OVZ安装完毕后敲入：
nohup /usr/local/net_speeder/net_speeder venet0 "ip" >/dev/null 2>&1 &

KVM安装完毕后敲入：
nohup /usr/local/net_speeder/net_speeder eth0 "ip" >/dev/null 2>&1 &
注意这里引号中的IP不需要动，有的地方说需要改成自己的IP地址，其实不用改！

查看 net-speeder 是否运行
ps aux|grep net_speeder|grep -v grep

关闭net_speeder：
killall net_speeder

指定端口加速的方法:
比如net_speeder venet0 "src port 22"就是针对22号端口流出的数据包加速。
加速多个端口：net_speeder venet0 "src port 22 || 80 || 443"

运行并加入开机启动
OVZ
echo "nohup /usr/local/net_speeder/net_speeder venet0 "ip" >/dev/null 2>&1 &" >> /etc/rc.local
KVM
echo "nohup /usr/local/net_speeder/net_speeder eth0 "ip" >/dev/null 2>&1 &" >> /etc/rc.local




