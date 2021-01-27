# Archlinux双系统安装教程

## 1.硬盘分区

可以使用DiskGenius，也可以使用Windows的磁盘管理。

![image5](/home/hany/Desktop/github/images/image5.png)

## 2.制作启动U盘

U盘镜像制作工具使用Rufus，写入方式为DD，分区类型选择GPT。

## 3.检查网络

U盘启动后检查网络环境。

```bash
[root@archiso]# ip a
```

网络连接可以有线连接，也可无线连接。

###### (1)有线连接，例如：

![image6](/home/hany/Desktop/github/images/image6.png)

###### (2)无线连接，方法：

依次输入下面命令

```bash
[root@archiso]# iwctl
[root@archiso]# device list
[root@archiso]# station wlan0 scan      查看网卡名字，假设是wlan0
[root@archiso]# station wlan0 get-networks      检查扫描网络
[root@archiso]# station wlan0 connect Tenda_C18965      查看网络名字，假设名字叫Tenda_C18965
[root@archiso]# exit      接着输入密码后退出iwd模式
```

无线连接，例如：

![image7](/home/hany/Desktop/github/images/image7.png)

无线连接成功之后继续输入下面命令

```bash
[root@archiso]# pacman -Syyy		更新源
[root@archiso]# nano /etc/pacman.d/mirrorlist		重新设置mirrorlist
```

**注意：设置mirrorlist需要将自己国家前的源用#注释掉，然后按ctrl+O保存，ctrl+x退出。</u>**

## 4. 硬盘分区

###### (1).检查硬盘

```bash
[root@archiso]# lsblk
```

```bash
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0 223.6G  0 disk 
├─sda1   8:1    0   824M  0 part 
├─sda2   8:2    0 122.3G  0 part 
sdb      8:16   0 931.5G  0 disk 
├─sdb1   8:17   0   499M  0 part 
├─sdb2   8:18   0   600G  0 part 
└─sdb3   8:19   0 130.9G  0 part
```

上图显示的是已经存在的分区，之前划分的空间并没有显示。

###### (2).建立分区

之前划好的空间在sda上，所以执行

```bash
[root@archiso]# cfdisk /dev/sda
```

```bash
                             disk：/dev/sda
                size：223.57 GiB，240057409536 bytes，468862128 sectors
               Label：gpt，identifier：F81DAEF6-4661-4B95-AD81-742D8D312E99

    Device                      Start               End             sectors           Size Type
    /dev/sda1                    2048            1689599            1687552           824M EFI System
    /dev/sda2                 1690080          258250638          256560559         122.3G Microsoft basic data 
>>  Free space              258250752          468862094          210611343         100.4G 


	[ New ]	       [ Quit ]	 		[ Help ]	 	[ Write ]		[ Dump ]
```

过程：1.选择New  	2.这里就输入100.4G  	3.选择Write ，输入 yes 回车	 4.写入完成，选择Quit回车退出

终端输入：lsblk	得到如图结果：

```bash
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0 223.6G  0 disk 
├─sda1   8:1    0   824M  0 part 
├─sda2   8:2    0 122.3G  0 part 
└─sda3   8:3    0 100.4G  0 part 
sdb      8:16   0 931.5G  0 disk 
├─sdb1   8:17   0   499M  0 part 
├─sdb2   8:18   0   600G  0 part 
└─sdb3   8:19   0 130.9G  0 part
```

###### (3).分区格式化

将刚刚分好的区格式化为ext4格式

```bash
[root@archiso]# mkfs.ext4 /dev/sda3
```

###### (4).挂载分区（包括root和EFI分区）

```bash
[root@archiso]# mount /dev/sda3 /mnt		挂载/分区
[root@archiso]# mkdir /mnt/boot		建立boot文件夹
[root@archiso]# mount /dev/sda1 /mnt/boot		挂载EFI分区
```

**注意：*<u>*Archlinux要与Windows共用EFI分区，由上图4(2)可以确定sda1是EFI分区，记下来后面要用。*</u>***

## 5.安装基本系统

输入并等待安装完成

```bash
[root@archiso]# pacstrap /mnt base linux linux-firmware nano
```

## 6. 生成fstab文件

```bash
[root@archiso]# genfstab -U /mnt >> /mnt/etc/fstab
```

## 7.正式配置新系统

```bash
[root@archiso]# arch-chroot /mnt		切换到装好的系统
[root@archiso]# fallocate -l 8GB /swapfile		建立swapfile
[root@archiso]# chmod 600 /swapfile		改权限
[root@archiso]# mkswap /swapfile		建立swap空间
[root@archiso]# swapon /swapfile		激活swap空间
[root@archiso]# nano /etc/fstab		修改/etc/fstab文件
```

**注意：swapfile分配的大小最好与硬件内存大小相同。**

在fstab文件末尾输入，保存并退出。

```bash
/swapfile none swap defaults 0 0   （是数字0而不是字母o）
```

![image8](/home/hany/Desktop/github/images/image8.png)

```bash
[root@archiso]# ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime		设置时区
[root@archiso]# hwclock --systohc			同步硬件时钟
[root@archiso]# nano /etc/locale.gen		设置locale，保存退出
```

注意：在文件locale.gen文件中找到 #en_US.UTF-8 UTF-8和#zh_CN.UTF-8 UTF-8 删掉前面的#（取消注释）

```bash
[root@archiso]# locale-gen		生成locale
[root@archiso]# nano /etc/locale.conf		创建并写入/etc/locale.conf文件
```

在locale.conf文件中填写 LANG=en_US.UTF-8 保存退出

![image10](/home/hany/Desktop/github/images/image10.png)

```bash
[root@archiso]# nano /etc/hostname		创建并写入hostname,其中hostname根据自己喜好填
[root@archiso]# nano /etc/hosts		修改hosts
```

hosts文件修改内容如下图，保存退出

![image9](/home/hany/Desktop/github/images/image9.png)



```bash
[root@archiso]# passwd		为root用户创建密码，输入并确认密码
[root@archiso]# pacman -S grub efibootmgr networkmanager network-manager-applet dialog wireless_tools wpa_supplicant os-prober mtools dosfstools ntfs-3g base-devel linux-headers reflector git sudo		安装基本包
[root@archiso]# grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=Arch		  安装grub
[root@archiso]# mkdir /mnt/windows10       为了让grub接管启动器之后还能正确地进入Windows系统
[root@archiso]# mount /dev/sda2 /mnt/windows10	挂载Windows系统所处的区，我这里是sda2
[root@archiso]# grub-mkconfig -o /boot/grub/grub.cfg		  生成grub.cfg
```

## 8.退出安装过程，取消挂载并重启	(启动时需拔出u盘)

```bash
[root@archiso]# exit
[root@archiso]# umount -a
[root@archiso]# reboot
```

## 9. 进入Arch系统并激活网络

进去之后 先输入 root 回车 输入密码，回车

```bash
[root@archiso]# systemctl enable --now NetworkManager		启动网络服务
[root@archiso]# nmtui		设置WiFi
```

选择你要连接到的WiFi 输入密码 回车 然后退出

## 10.新建用户并授权

```bash
[root@archiso]# useradd -m -G wheel arch
```

wheel后面是你的用户名，这里输入的是arch

```bash
[root@archiso]# passwd arch		  为用户创建密码
[root@archiso]# EDITOR=nano visudo		  授权
```

**注意：Ctrl+W 输入 # %wheel <u>(#与%之间有一个空格)</u>回车，找到这行， 删除前面的 #（取消注释），保存退出**

## 11.安装显卡驱动

```bash
[root@archiso]# pacman -S xf86-video-amdgpu		  安装AMD集显驱动
[root@archiso]# pacman -S nvidia nvidia-utils		安装NVIDIA独显驱动
```

## 12.安装Display Server

```bash
[root@archiso]# pacman -S xorg		使用的是xorg，出现选择直接回车即可
```

## 13.安装Display Manager

这里根据自己的喜好安装的桌面环境，常用有如下:

```bash
[root@archiso]# pacman -S gdm		Gnome桌面
[root@archiso]# pacman -S sddm		 KDE桌面
[root@archiso]# pacman -S lightdm lightdm-gtk-greeter		Xfce || DDE桌面
```

设置开机自动启动，以sddm为例：

```bash
[root@archiso]# systemctl enable sddm
```

## 14.安装Desktop Environment

```bash
[root@archiso]# pacman -S gnome		  Gnome桌面，也可以另外再安装gnome-extra
[root@archiso]# pacman -S plasma kde-applications packagekit-qt5		KDE桌面
[root@archiso]# pacman -S xfce4 xfce4-goodies		Xfce桌面
[root@archiso]# pacman -S deepin deepin-extra		DDE桌面
```

需要选择时直接回车

## 15.安装完成

等待安装完毕,重启

```bash
[root@archiso]# reboot
```

当看到登录界面时，一个相对完整的Arch安装完毕！



主要参考：

[Youtube](https://www.youtube.com/watch?v=QU0KRTgRJZU&t=14s)

[2020 Archlinux双系统安装教程（超详细）](https://zhuanlan.zhihu.com/p/138951848)

[博客园](https://www.cnblogs.com/xwylog/p/10963589.html)