# 一、在grub的rescue模式修复linux引导

#### （一）进入系统

1、查找系统引导所在的分区：

```
grub rescue> ls
(hd0) (hd1) (hd1,gpt8)....
例如：grub rescue> ls (hd1,gpt4)/**/***
寻找系统安装的分区，有输出目录的分区即为所需要的分区。
```

2、配置grub引导

```
grub rescue> set root=(hd1,gpt4)
grub rescue> set prefix=(hd1,gpt4)/boot/grub
```

3.切换到normal模式

```
grub rescue> insmod normal
grub rescue> normal
```

#### （二）安装grub到efi分区

1.找到系统efi分区挂载点

运行df命令：

如下图可看出efi挂载点是：/dev/sda1

![image](https://github.com/zjkhy94/manjaro-linux-/blob/main/images/image1.png)

2.重新安装grub

```
sudo update-grub
sudo grub-install /dev/sda1
```

如果安装成功，会显示输出：Installation finished. No error reported.

linux引导修复到此结束，可以重启尝试是否问题解决。
