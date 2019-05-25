# netspeeder-onekeyinstall  
ALL COPY FROM IMEIJI  
CentOS一键安装  

wget --no-check-certificate https://raw.githubusercontent.com/wjk199511140034/netspeeder-onekeyinstall/master/net_speeder_lazyinstall.sh  
bash net_speeder_lazyinstall.sh  

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


# 自动检测运行  
由于netspeeder执行双倍发包，如果全天24小时运行势必造成流浪浪费，只需要设置在晚高峰期运行即可  
首先安装crond服务，并设置开机自启(一般系统已自带并且设置开机自启)

创建检查运行脚本：  
touch /root/net_speeder_start.sh

写入内容：  
vi /root/net_speeder_start.sh

按下insert键，然后输入以下内容：  
#!/bin/bash  
if [[ -z `ps -A | grep net_speeder` ]]; then  
  nohup /usr/local/net_speeder/net_speeder eth0 "ip" >/dev/null 2>&1 &  
fi  
按下esc键，输入:wq退出vi编辑器

设置crontab：  
crontab -e

按下insert键，然后输入以下内容：   
*/5 20-23,0-2 * * * bash /root/net_speeder_start.sh  
01 03 * * * killall net_speeder  
按下esc键，输入:wq退出vi编辑器

可能还需要运行一下 service crond reload

- 说明：每天晚八点至凌晨三点之间，每隔五分钟检查一次netspeeder是否运行，如果没有运行则启动netspeeder，凌晨三点零一分杀掉netspeeder进程







