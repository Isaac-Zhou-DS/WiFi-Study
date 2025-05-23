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
#### 方案1---适合长时间抓包
(1)使用MobaXterm通过SSH登录open-wrt root界面：用户名：192.168.5.1 password：password

(2)进入root界面后，按如下命令操作

1.执行命令"ifconfig"查看网卡配置,并找到open-wrt所对应的无线网卡，例如：phy0-mon0 

2.执行命令"iw phy phy0 interface add mon0 type monitor"将无线网卡功能定义为monnitor，网卡名mon0；

3.执行命令"ifconfig mon0 up"enable 无线网卡mon0

4.设置目标channle

uci show wireless

uci set wireless.radio0.channel='11'

uci commit wireless

wifi #使配置生效

5.在windows cmd界面输入命令"ssh root@192.168.5.1 "tcpdump -i mon0 -w - " > D:\capture.pcap"  该命令在pc cmd界面操作，可保存到pc某个具体文件夹中，此方式适合需要长期抓取报文时使用，注意下发命令后需要在cmd界面输入root的password，这里需要解决此问题；

  也可执行命令"ssh root@192.168.5.1 "tcpdump -i mon0 -w - " | wireshark -k -i -"   该命令在pc cmd界面操作，直接在wireshark实时显示抓取的报文，注意下发命令后需要在cmd界面输入root的password，这里需要解决此问题；
#### 方案2---适合短时间抓包
(1)使用MobaXterm通过SSH登录open-wrt root界面：用户名：192.168.5.1 password：password

(2)进入root界面后，按如下命令操作

1.执行命令"ifconfig"查看网卡配置,并找到open-wrt所对应的无线网卡，例如：phy0-mon0 

2.执行命令"iw phy phy0 interface add mon0 type monitor"将无线网卡功能定义为monnitor，网卡名mon0，注意之前已经创建过了，可以不执行此命令；

3.执行命令"ifconfig mon0 up"enable 无线网卡mon0

4.设置目标channle

uci show wireless

uci set wireless.radio0.channel='11'

uci commit wireless

wifi #使配置生效

5.安装rpcapd

并执行命令："opkg update" "opkg install rpcapd"，若已安装忽略此操作

5.执行命令"rpcapd -4 -n -d",运行rpcapd；

6.打开wireshark，做如下配置：

![image](https://github.com/user-attachments/assets/7e757ae1-e1d5-420b-b699-dc85b3dcbeca)


![image](https://github.com/user-attachments/assets/3dffb9e6-05be-4bd0-9e32-63995ec371d2)

主机：即open-wrt的ip：192.168.5.1

端口：默认2002

选择密码认证，用户命：root 密码：open-wrt的密码

此方案需要解决ssh频繁断连的问题；

### 3.修改open-wrt wireless参数
修改wireless参数：如channel，ssid，mode等

uci show wireless

uci set wireless.radio0.channel='11' 需要抓取指定信道时需要此命令

uci set wireless.radio0.mode="monitor" 修改mode为monitor

uci commit wireless

wifi---使配置生效
### 参考链接
https://blog.csdn.net/wu1013251335/article/details/141393094
