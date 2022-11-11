# vmware虚拟机网络配置

## 一、Bridged（桥接模式）

```base
什么是桥接模式？桥接模式就是将主机网卡与虚拟机虚拟的网卡利用虚拟网桥进行通信。在桥接的作用下，类似于把物理主机虚拟为一个交换机，所有桥接设置的虚拟机连接到这个交换机的一个接口上，物理主机也同样插在这个交换机当中，所以所有桥接下的网卡与网卡都是交换模式的，相互可以访问而不干扰。在桥接模式下，虚拟机ip地址需要与主机在同一个网段，如果需要联网，则网关与DNS需要与主机网卡一致。其网络结构如下图所示：
```

![桥接模式](https://img-blog.csdn.net/20160408183817187)





接下来，我们就来实际操作，如何设置桥接模式。

首先，安装完系统之后，在开启系统之前，点击“编辑虚拟机设置”来设置网卡模式。

![设置网卡模式](https://img-blog.csdn.net/20160408184030522)

点击“网络适配器”，选择“桥接模式”，然后“确定”

![网络适配器](https://img-blog.csdn.net/20160408184101860)

在进入系统之前，我们先确认一下主机的ip地址、网关、DNS等信息。

![主机信息](https://img-blog.csdn.net/20160408184145064)

然后，进入系统编辑网卡配置文件，命令为vi /etc/sysconfig/network-scripts/ifcfg-eth0

![编辑网卡配置文件](https://img-blog.csdn.net/20160408184217080)

添加内容如下：

![添加内容](https://img-blog.csdn.net/20160408184254049)

编辑完成，保存退出，然后重启虚拟机网卡，使用ping命令ping外网ip，测试能否联网。



能ping通外网ip，证明桥接模式设置成功。

那主机与虚拟机之间的通信是否正常呢？我们就用远程工具来测试一下。
![image](https://user-images.githubusercontent.com/83051290/201335753-5228e948-5242-4054-bbdc-f86a7a252d17.png)



主机与虚拟机通信正常。

这就是桥接模式的设置步骤，相信大家应该学会了如何去设置桥接模式了。桥接模式配置简单，但如果你的网络环境是ip资源很缺少或对ip管理比较严格的话，那桥接模式就不太适用了。如果真是这种情况的话，我们该如何解决呢？接下来，我们就来认识vmware的另一种网络模式：NAT模式。

## 二、NAT（地址转换模式）
```base
刚刚我们说到，如果你的网络ip资源紧缺，但是你又希望你的虚拟机能够联网，这时候NAT模式是最好的选择。NAT模式借助虚拟NAT设备和虚拟DHCP服务器，使得虚拟机可以联网。其网络结构如下图所示：
```
![image](https://user-images.githubusercontent.com/83051290/201335909-9be011fd-0959-453b-8ec8-a5acb664eabb.png)


在NAT模式中，主机网卡直接与虚拟NAT设备相连，然后虚拟NAT设备与虚拟DHCP服务器一起连接在虚拟交换机VMnet8上，这样就实现了虚拟机联网。那么我们会觉得很奇怪，为什么需要虚拟网卡VMware Network Adapter VMnet8呢？原来我们的VMware Network Adapter VMnet8虚拟网卡主要是为了实现主机与虚拟机之间的通信。在之后的设置步骤中，我们可以加以验证。

首先，设置虚拟机中NAT模式的选项，打开vmware，点击“编辑”下的“虚拟网络编辑器”，设置NAT参数及DHCP参数。
![image](https://user-images.githubusercontent.com/83051290/201335951-594a6050-6726-49be-ac33-215765168bbe.png)

![image](https://user-images.githubusercontent.com/83051290/201335968-42e8140a-eb79-43f7-9b29-c42689cac0ce.png)

![image](https://user-images.githubusercontent.com/83051290/201335986-47980a55-4c1f-4966-9eb0-f27a2eb954e9.png)





将虚拟机的网络连接模式修改成NAT模式，点击“编辑虚拟机设置”。

![image](https://user-images.githubusercontent.com/83051290/201336025-0dca320e-26b1-486e-8cb5-43c871789d00.png)


点击“网络适配器”，选择“NAT模式”

![image](https://user-images.githubusercontent.com/83051290/201336003-bc4075b5-6247-4a48-9c34-10eeec544b3d.png)


然后开机启动系统，编辑网卡配置文件，命令为vi /etc/sysconfig/network-scripts/ifcfg-eth0

![image](https://user-images.githubusercontent.com/83051290/201336057-25710947-3c77-436b-a619-a552d0174cf1.png)


具体配置如下：

![image](https://user-images.githubusercontent.com/83051290/201336070-eb8477dc-dc03-4ab3-8fc1-0c98abc50a1b.png)


编辑完成，保存退出，然后重启虚拟机网卡，动态获取ip地址，使用ping命令ping外网ip，测试能否联网。

![image](https://user-images.githubusercontent.com/83051290/201336089-6305d740-6e1b-421c-8fc2-cd93354e5deb.png)


之前，我们说过VMware Network Adapter VMnet8虚拟网卡的作用，那我们现在就来测试一下。

![image](https://user-images.githubusercontent.com/83051290/201336107-fc8e84fb-a986-4c3b-92b3-3d25f92d84d3.png)

![image](https://user-images.githubusercontent.com/83051290/201336122-41dfc213-45e6-4c45-bfb2-857aa4ecd1ea.png)



如此看来，虚拟机能联通外网，确实不是通过VMware Network Adapter VMnet8虚拟网卡，那么为什么要有这块虚拟网卡呢？

之前我们就说VMware Network Adapter VMnet8的作用是主机与虚拟机之间的通信，接下来，我们就用远程连接工具来测试一下。

![image](https://user-images.githubusercontent.com/83051290/201336144-626f3459-f20c-4e8e-bd3a-002190f85c1b.png)


然后，将VMware Network Adapter VMnet8启用之后，发现远程工具可以连接上虚拟机了。

那么，这就是NAT模式，利用虚拟的NAT设备以及虚拟DHCP服务器来使虚拟机连接外网，而VMware Network Adapter VMnet8虚拟网卡是用来与虚拟机通信的。

## 三、Host-Only（仅主机模式）
```base
Host-Only模式其实就是NAT模式去除了虚拟NAT设备，然后使用VMware Network Adapter VMnet1虚拟网卡连接VMnet1虚拟交换机来与虚拟机通信的，Host-Only模式将虚拟机与外网隔开，使得虚拟机成为一个独立的系统，只与主机相互通讯。其网络结构如下图所示：
```
![image](https://user-images.githubusercontent.com/83051290/201336189-a6c05323-b397-427d-a711-e7e667c0abac.png)


通过上图，我们可以发现，如果要使得虚拟机能联网，我们可以将主机网卡共享给VMware Network Adapter VMnet1网卡，从而达到虚拟机联网的目的。接下来，我们就来测试一下。

首先设置“虚拟网络编辑器”，可以设置DHCP的起始范围。

![image](https://user-images.githubusercontent.com/83051290/201336210-56a51dd8-787f-435b-95fa-1a82d0ef3dec.png)


设置虚拟机为Host-Only模式。

![image](https://user-images.githubusercontent.com/83051290/201336270-9088251a-77e3-4966-8206-ea28b9e939e9.png)


开机启动系统，然后设置网卡文件。

![image](https://user-images.githubusercontent.com/83051290/201336294-0a5d019c-c12e-4f54-a849-25a891e9421a.png)


保存退出，然后重启网卡，利用远程工具测试能否与主机通信。

![image](https://user-images.githubusercontent.com/83051290/201336317-982864a4-4ee5-46d0-ac06-182c47a2f5b2.png)


主机与虚拟机之间可以通信，现在设置虚拟机联通外网。

![image](https://user-images.githubusercontent.com/83051290/201336337-b3d0e66e-3959-4d27-9974-96aa3538bdf1.png)


我们可以看到上图有一个提示，强制将VMware Network Adapter VMnet1的ip设置成192.168.137.1，那么接下来，我们就要将虚拟机的DHCP的子网和起始地址进行修改，点击“虚拟网络编辑器”

![image](https://user-images.githubusercontent.com/83051290/201336349-57a021d5-4d40-4475-b585-320e7d54e715.png)



重新配置网卡，将VMware Network Adapter VMnet1虚拟网卡作为虚拟机的路由。

![image](https://user-images.githubusercontent.com/83051290/201336360-32dfbecb-aff8-4a31-ae60-582450d8c24a.png)


重启网卡，然后通过 远程工具测试能否联通外网以及与主机通信。

![image](https://user-images.githubusercontent.com/83051290/201336377-bd606aee-5c9d-4441-be70-70d90b493514.png)


测试结果证明可以使得虚拟机连接外网

