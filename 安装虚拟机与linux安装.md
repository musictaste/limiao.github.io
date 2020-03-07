
1.安装包和镜像下载
VMWare workstation下载地址：
https://my.vmware.com/cn/web/vmware/info/slug/desktop_end_user_computing/vmware_workstation_pro/15_0
![Image text](https://raw.githubusercontent.com/musictaste/java/master/image/1.png)
linux镜像下载地址：http://archive.kernel.org/centos-vault/6.5/isos/x86_64/
选择CentOS-6.5-x86_64-minimal.iso  

2.安装（虚拟环境）
2.1 安装VMware,简单，点下一步就行
2.2 VMWare workstation创建虚拟机
采用“自定义”类型配置
选择硬件兼容性：workstation 12.x
稍后安装操作系统
版本选择centOS 6 64位
修改虚拟机名称
虚拟机内存为：1G
使用NAT网络
100G硬盘



















2.3.安装linux操作系统
选择CD/DVD,设置使用ISO镜像文件
开启虚拟机
选择上海
自定义分区
reboot
















自定义分区基础知识：
sda
1.boot 引导程序区
    用于linux内核启动
    ext4 200M
2.swap交换区
    内存-->交换区中
    app  内存不足，启用一个进程把部分内存写到交换区中
    资源紧张的情况下，运行多个进程的保障

    swap   2048M   默认为内存的2倍
3.用户区
    设置剩余的磁盘空间都挂在用户区

sdb第二块磁盘
sdc第三块磁盘
线性显示


2.4.配置虚拟环境的网络
2.4.1.找到网卡位置
cd /etc/sysconfig/network-scripts

知识点：
ifcfg-eth0文件
if:interface
cfg:config
eth:以太网
0：第0块网卡

2.4.2.配置协议
vi ifcfg-eth0
1.删除虚拟网卡物理地址HWADDR、UUID：方便后期克隆虚拟机，不至于多个虚拟机之间有相同的网卡物理地址，防止出现网络问题

知识点：
DEVICE:设备名
ONBOOT=yes 代表网卡是否启用
BOOTPROTO=static  采用静态配置IP
BOOTPROTO=dhcp:动态设置Ip

nat模式，子网iP：192.168.10.0  
0代表网络号
255代表广播地址
可用的ip地址是0到254


nat设置：网关192.168.10.2
ip=2不能用，因为被占用了


对勾：将主机虚拟适配器连接到此网络，勾选以后，多一个虚拟适配器
ip地址为：192.168.10.1，ip=1也被占用了


2.配置IPADDR、NETMASK、GATEWAY、DNS1、DNS2
修改参数
ONBOOT=yes 
BOOTPROTO=static  

增加参数
IPADDR=192.168.10.66
NETMASK=255.255.255.0
GATEWAY=192.168.10.2
DNS1=114.114.114.114
DNS2=192.168.10.2


注：截图中有问题，需要删除HWADDR参数


2.4.3.重启网络服务，这样你修改的配置信息才能生效
service network restart
2.4.4.测试：ping www.baidu.com




4.基于已经配置好的虚拟机来克隆多台虚拟机，为学习大数据做准备
4.1.修改IP地址
vi /etc/sysconfig/network-scripts/ifcfg-eth0
修改ip为其他没有使用的ip地址，另外注意是否还有HDADDR配置，如果有，一同删掉

4.2.hostname
vi /etc/sysconfig/network
修改hostname


4.3.删除一个文件
rm -f /etc/udev/rules.d/70-persistent-net.rules
因为这个文件中保存了网卡的物理地址与网卡名的关系
自动生成一个新的文件




===========================================================================
补充知识：
vi进入编辑模式：i
退出：esc      shift+;     wq!
或者esc shift+zz
或者esc x 


编辑完按Esc退出编辑模式此时输入：
：wq保存后退出
：wq!强制保存后退出
：w保存但不退出
:q不保存并退出
：q！不保存并强制退出


拍摄快照：右键-快照-拍摄快照

克隆：快照管理器中选择快照进行克隆，选择“创建链接克隆”


遇到的问题：
1.VMware安装虚拟机，选择CentOS 6 64位，提示“此主机不支持64位客户机操作系统”
方法一：按照百度的内容，重启电脑进入BIOS，找到Intel Virtualization Technology，选择Enabled，保存并重启，----不好使，因为我本机IVT就是enabled，当然设置了也没有效果
解决方法：选择硬件兼容性：workstation 由15.x修改为workstation 12.x，可以了

2.
总结：
1.VMWare workstation创建虚拟机
2.安装linux操作系统
自定义分区sda
1.boot 引导程序区
2.swap交换区
3.用户区

3.配置虚拟环境的网络
1.找到网卡位置，修改协议配置
1）.删除虚拟网卡物理地址HWADDR、UUID
2）.配置IPADDR、NETMASK、GATEWAY、DNS1、DNS2
2.重启网络服务
3.测试：ping www.baidu.com

4.基于已经配置好的虚拟机来克隆多台虚拟机
1.修改IP地址
2.修改Hostname
3.删除一个文件
