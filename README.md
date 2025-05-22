# WiFi 抓包环境搭建(open-wrt篇)
## 一、涉及的工具
MobaXterm,WireShark,open-wrt
## 二、抓包原理
本方法主要使用tcpdump功能进行抓包。
## 三、抓包步骤
### 1.环境搭建
(1) 需要刷机的open-wrt一台，网线两根，USB转网口,安装MobaXterm，WireShark  

(2) 拓扑图如下：
### 2.操作步骤
#### 方案1
(1)使用MobaXterm通过SSH登录open-wrt root界面：用户名：192.168.5.1 password：password

(2)进入root界面后，按如下命令操作

1.执行命令"ifconfig"查看网卡配置,并找到open-wrt所对应的无线网卡，例如：phy0-mon0 

2.执行命令"iw phy phy0 interface add mon0 type monitor"将无线网卡功能定义为monnitor，网卡名mon0；
：
3.执行命令"ifconfig mon0 up"enable 无线网卡mon0

4.在windows cmd界面输入命令"ssh root@192.168.5.1 "tcpdump -i mon0 -w - " > D:\capture.pcap"  该命令在pc cmd界面操作，可保存到pc某个具体文件夹中，此方式适合需要长期抓取报文时使用，注意下发命令后需要在cmd界面输入root的password，这里需要解决此问题；

  也可执行命令"ssh root@192.168.5.1 "tcpdump -i mon0 -w - " | wireshark -k -i -"   该命令在pc cmd界面操作，直接在wireshark实时显示抓取的报文，注意下发命令后需要在cmd界面输入root的password，这里需要解决此问题；
### 3.修改open-wrt wireless参数
修改wireless参数：如channel，ssid，
uci show wireless

uci set wireless.radio0.channel='11'

uci commit wireless

wifi---使配置生效
