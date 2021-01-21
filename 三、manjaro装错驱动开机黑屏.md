# 三、manjaro装错驱动开机黑屏

问题描述：

关机前安装了video-vesa驱动后，开机黑屏，如图所示。
![image4](https://github.com/zjkhy94/manjaro-linux-zjkhy94/blob/main/images/image4.png)

问题解决：

开机后，在黑屏界面下按Ctrl+Alt+F3进入tty3输入密码进入系统，然后输入命令 `mhwd -li`

```
> Installed PCI configs:
--------------------------------------------------------------------------
            NAME               VERSION          FREEDRIVER           TYPE
--------------------------------------------------------------------------
      video-vesa            2017.03.12                true            PCI
     video-linux            2018.05.04                true            PCI
video-modestting            2020.01.13                true            PCI

Warning: No installed USB configs!
```

然后输入：

```
sudo mhwd -f -r pci video-vesa
```

重新启动，问题解决。
