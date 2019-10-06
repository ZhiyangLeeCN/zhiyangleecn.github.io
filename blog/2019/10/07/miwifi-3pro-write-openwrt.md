---
layout: default
title: 小米路由器Pro刷OpenWrt固件
---

#前言
小米路由器刷机的资料感觉相对其它厂家的路由器还是比较少的，而且小米家路由器仅有的资料又和我这个路由器型号(我的是小米路由器Pro)对不上，但庆幸的是我在OpenWrt的官网上找到了对应型号的文档，所以整个刷机过程也比较顺利。

#小米路由器的前置条件
首先要把小米路由器的固件升级为开发版本，因为后面获取路由器的SSH功能需要，首先进入小米开发固件的官方下载页面:[http://www1.miwifi.com/miwifi_download.html](http://www1.miwifi.com/miwifi_download.html)

![](https://upload-images.jianshu.io/upload_images/2133834-47945ea493c49de5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
选择"ROM"后，在下面找到自己路由器对应型号的开发版本固件并下载(我的是小米路由器Pro):
![](https://upload-images.jianshu.io/upload_images/2133834-c4c7a20136dc632c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后进入到小米路由器的管理后台，在右上角的下拉菜单中选择“系统升级”:
![](https://upload-images.jianshu.io/upload_images/2133834-6f5d368079fd30b6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击手动升级:
![](https://upload-images.jianshu.io/upload_images/2133834-2f24ad3b5c2bd769.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后选择你前面下载下来的开发版固件包，并点击开始升级，注意，后面会提示让你清空现有路由器的配置，一般建议清空，为了防止当前配置和你下载的固件包版本不兼容导致一些问题出现，清空后相当于reset初始化，需要你在重新设置一遍路由器:
![](https://upload-images.jianshu.io/upload_images/2133834-54df02b57caf0394.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

随后会有大约8~10分钟的安装过程，这个过程路由器的指示灯是黄色的，等待变为蓝色则是安装完成，当安装成功后，这个时候再回到路由器状态页面时，系统ROM版本那一栏应该显示为“MIWIFI 开发版本 X.X.X”。

随后需要你初始化路由器，并用小米WIFI（自行搜索下载）APP绑定你的这台路由器，绑定成功后进到:[https://d.miwifi.com/rom/ssh](https://d.miwifi.com/rom/ssh) 这个页面下载你对应路由器的SSH包文件:
![](https://upload-images.jianshu.io/upload_images/2133834-4105c5882760a3a1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

到这里你需要准备一个U盘，而且U盘的格式要为FAT或者FAT32，然后将你下载的这个工具包文件放在U盘根目录(不在任何子目录下)，然后将路由器电源拔掉后在插入该U盘，U盘插入后再按住路由器的reset键（小米路由器3Pro需要牙签之类的才能按得到），在按住不松开reset键的同时重新把路由器电源接上，等待路由器前面的指示灯变成黄色一闪一闪的时候就可以松开了，这个时候等待指示灯变蓝，就代表SSH获取成功了。

如果你在路由器接着电源的时候把U盘插上路由器了，路由器会把你的U盘设置为外接存储设备，这会导致你按照前面的步骤操作不会成功，这个时候把U盘重新格式化一遍(一定要是FAT/FAT32格式，其它格式如NTFS或者exFAT也是不会成功的)，在把工具包文件拷贝进去重来一遍即可。WIN10的系统不能格式U盘为FAT/FAT32格式，这个时候可以用第三方工具如:DiskGenius等工具去操作。

Windows下可以使用ipconfig查看默认网关地址，也就是路由器地址，例如我的路由器地址为:192.168.28.1。一般Win10都自带了SSH工具，可以直接打开命令提示符后使用以下命令进行连接验证:

```
 /*192.168.28.1替换成你自己路由器的地址*/
 ssh root@192.168.28.1
```

如果SSH没获取成功一般大概率会提示你“connection refused”，而如果成功则开始进入密码输入环节(密码在上面那个下载SSH工具包的页面中可以看到)，成功后的界面如下:
![](https://upload-images.jianshu.io/upload_images/2133834-85e51f83835ee5bf.JPG?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



#下载对应的OpenWrt固件
最好的情况下是直接找到路由器型号对应的OpenWrt固件(因为自己编译又耗费精力又耗费时间)，一般可以先在OpenWrt官方的硬件文档中寻找到自己对应的路由器:[https://openwrt.org/toh/start](https://openwrt.org/toh/start) 在这个页面中ctrl+f搜索“XiaoMi”:
![](https://upload-images.jianshu.io/upload_images/2133834-d703d6fe8db7a16d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
可以点击最后一列的“ViewEdit data”来查看该硬件设备的文档，这里面会包含该硬件型号可用的OpenWrt固件文件的下载地址:
![](https://upload-images.jianshu.io/upload_images/2133834-821cf96433b4cb12.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
当你的机器时第一次安装的时候，一般使用factory后缀的固件包，而如果你的机器已经安装过该固件包只是升级则使用sysupgrade后缀的固件包，而且一般还会有tftp后缀的固件包，这个通常是刷出问题来了后需要恢复时的固件包。

这里我们的机器是第一次安装，所以下载factory后缀的固件包。当下载后可以选择使用scp拷贝到路由器的/tmp/目录下，也可以拷至U盘的根目录中，在插到路由器上使用。

我这里使用的scp拷贝方式:
```
//E:\Download\XXXX 是我本地的保存路径，替换成你自己的即可
 scp E:\Download\openwrt-ramips-mt7621-xiaomi_mir3p-squashfs-factory.bin root@192.168.28.1:/tmp/ 
```

最后登入SSH，如果你使用的是scp方式拷贝，则步骤如下:
1. 进入到固件包文件所在位置
``` 
cd /tmp/
 ```
2. 重命名一个短文件名
``` 
mv openwrt-ramips-mt7621-xiaomi_mir3p-squashfs-factory.bin factory.bin
```

如果你使用的U盘方式，则步骤如下:
1.进入到固件包文件U盘所在位置
```
 cd /extdisks/sda1
 ```

2. 重命名一个短文件名
``` 
mv openwrt-ramips-mt7621-xiaomi_mir3p-squashfs-factory.bin factory.bin
```

最后按照以下命令顺序执行刷入固件:
```
nvram set flag_try_sys1_failed=1 
nvram set flag_try_sys2_failed=0 
nvram set flag_boot_success=0 
nvram commit
dd if=factory.bin bs=1M count=4 | mtd write - kernel1
mtd erase rootfs0
mtd erase rootfs1
mtd erase overlay
dd if=factory.bin bs=1M skip=4 | mtd write - rootfs0
reboot
```
随后路由器进入重启状态，等待前面的指示灯由黄色进入蓝色则代表安装成功，此过程一般需要等待几分钟，OpenWrt的默认网关地址一般为:192.168.1.1，一般默认情况下是不带WEB UI界面的，所以你要先登录进SSH安装UI界面。
```
ssh root@192.168.1.1 #默认没有密码

opkg update #次过程可能会因网络有失败，如果有失败重新在执行一遍等成功即可(因为会影响到下面命令的执行)
opkg install luci #安装WEB UI界面
```
安装成功后，则可以直接访问192.168.1.1进入WEB UI的管理界面:
![](https://upload-images.jianshu.io/upload_images/2133834-ad4e62580ff87518.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)