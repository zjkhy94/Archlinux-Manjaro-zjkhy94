# 二、Archlinux软件安装及优化

### (一)常用软件(该部分和Manjaro基本相似)

###### 1、添加archlilnuxcn源（arch中文社区的软件源仓库）

首先安装vim：

```
sudo pacman -S vim
```

安装结束后编辑/etc/pacman.conf

```
sudo vim /etc/pacman.conf
```

在末尾添加：

```
[archlinuxcn]
SigLevel = Optional TrustedOnly
Server = http://mirrors.tuna.tsinghua.edu.cn/archlinux-cn/$arch
****************************************************************
[archlinuxcn]
SigLevel = Optional TrustedOnly
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
```

**<u>切忌：二者选其一！</u>**

修改文件后，执行下面的命令:

```
sudo pacman -Syy && sudo pacman -S archlinuxcn-keyring
```

###### 2、yay

yay是下一个最好的 AUR 助手。它使用 Go 语言写成，宗旨是提供最少化用户输入的 `pacman` 界面、yaourt 式的搜索，而几乎没有任何依赖软件。

安装 yay：

```
sudo pacman -S yay
```

使用 yay：

设置中国源：

```
yay --aururl "https://aur.tuna.tsinghua.edu.cn" --save
```

搜索：

```
yay -Ss <package-name>
```

安装：

```
yay -S <package-name>
```

###### 3、安装输入法 (搜狗输入法)

```
sudo pacman -S fcitx-im
sudo pacman -S fcitx-configtool
yay -S fcitx-sogoupinyin
```

添加输入法配置文件 (sudo vim ~/.xprofile)

```
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```

###### 4、安装chrome

```
yay -S google-chrome
```

###### 5、Markdown编辑器：Typora

```
sudo pacman -S typora
```

###### 6、安装百度网盘

```
yay -S baidunetdisk-bin
```

###### 7、安装wps

```
sudo pacman -S wps-office ttf-wps-fonts
```

###### 8、安装debtap

(1)安装

```
yay -S debtap
```

(2)升级

(2.1)替换源，解决debtap同步仓库国内执行超慢

打开 /usr/bin/debtap，更换 debtap内容：

```shell
替换：http://ftp.debian.org/debian/dists
https://mirrors.ustc.edu.cn/debian/dists

替换：http://archive.ubuntu.com/ubuntu/dists
https://mirrors.ustc.edu.cn/ubuntu/dists/
```

(2.2)执行升级

```
sudo debtap -u
```

(3)使用方法

```
sudo debtap xxxx.deb
```

注意： 安装时会提示输入包名，以及license。包名随意，license就填GPL吧 上述操作完成后会在deb包同级目录生成xxx.tar.xz文件

(4)静默模式

-q 略过除了编辑元数据之外的所有问题，-Q略过所有的问题。

```
debtap -q xxx.deb**********debtap -Q xxx.deb
```

(5)安装转换好的本地包

```
sudo pacman -U xxx.tar.xz
```

###### 9、安装todesk

```
yay -S todesk
```

###### 10、安装网易云音乐

```
sudo pacman -S netease-cloud-music
```

###### 11、安装Foxit

```
yaourt foxit
```

###### 12、安装filezilla 

```
sudo pacman -S filezilla
```

###### 13、安装screenfetch (在终端里输出你的系统logo和状态)

```
pacman -S screenfetch
```

要让screenfetch在打开终端是自动输出:

```
vim ~/.bashrc
```

然后在最后添加：

```
screenfetch
```

###### 14.安装Deepin-wechat、Deepin-TIM

```
yay -S com.qq.weixin.spark******(Deepin-wechat)
yay -S com.qq.tim.spark******(Deepin-tim)
```

###### 15.字体安装（Windows字体）

```
sudo cp * /usr/share/fonts
sudo mkfontscale
sudo mkfontdir
sudo fc-cache
```

也可以用自带的字体管理安装。

###### 16.安装系统备份软件Timeshift

```
sudo pacman -S timeshift
```

(1)按照需求备份

(2)系统还原

<1>.如果可以进入系统，直接打开Timeshift还原。

<2>.如果只能进入登录界面。

[1].通过Ctrl+Alt+F2进入tty终端。

[2].输入用户和密码登录。

[3].执行下面命令获取系统当前可以还原的节点：

```
sudo timeshift --snapshot-device /dev/sdb4
```

```
Device : /dev/sdb4
UUID   : 03bbad1d-16eb-4f1c-b3eb-b5be8a2c0f1d
Path   : /run/timeshift/backup
Mode   : RSYNC
Status : OK
12 snapshots, 128.0 GB free

Num     Name                 Tags   Description  
------------------------------------------------------------------------------
0    >  2021-01-26_14-11-46  O                   
1    >  2021-01-26_19-00-01  D W M               
2    >  2021-01-26_23-29-48  O                   
3    >  2021-01-27_19-00-01  D                   
4    >  2021-01-28_15-10-17  O                   
5    >  2021-01-28_22-00-02  D                   
6    >  2021-01-29_17-58-12  O                   
7    >  2021-01-29_22-00-01  D                   
8    >  2021-01-30_22-00-02  D                   
9    >  2021-01-31_18-19-17  O                   
10   >  2021-02-02_10-00-01  H                   
11   >  2021-02-02_10-42-35  O    
```

[4].选择一个节点进行还原

```
sudo timeshift --restore --snapshot '2021-02-02_10-00-01' --skip-grub
```

<3>.如果无法进入系统，需要通过U盘启动系统

进入Live系统后打开 **Timeshift** 软件进行还原。

###### 17.vscode安装与配置

```
sudo pacman -S visual-studio-code-bin
```

[1].安装必要的插件

- C/C++
- Code Runner
- Chinese (Simplified) Language Pack for Visual Studio Code

[2].配置调试环境

**launch.json**

```
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "C/C++",
            "type": "cppdbg",
            "request": "launch",
            "program": "${fileDirname}/${fileBasenameNoExtension}",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "preLaunchTask": "compile",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ]
        }
    ]
}
```

**tasks.json**

```
{
    "version": "2.0.0",
    "tasks": [{
            "label": "compile",
            "command": "g++",		//如果需要是c语言也就是gcc将下面的command项由g++改为gcc
            "args": [
                "-g",
                "${file}",
                "-o",
                "${fileDirname}/${fileBasenameNoExtension}"
            ],
            "problemMatcher": {
                "owner": "cpp",
                "fileLocation": [
                    "relative",
                    "${workspaceRoot}"
                ],
                "pattern": {
                    "regexp": "^(.*):(\\d+):(\\d+):\\s+(warning|error):\\s+(.*)$",
                    "file": 1,
                    "line": 2,
                    "column": 3,
                    "severity": 4,
                    "message": 5
                }
            },
            "group": {
                "kind": "build",
                "isDefault": true
            }
        }
    ]
}
```

[3].去除窗口标题框

在 Vscode 设置中找到 `Title Bar Style`，将值改为 `custom`

### (二)遇到的问题及解决

###### 1.开启蓝牙

```
sudo pacman -S bluez bluez-utils bluedevil
sudo systemctl enable bluetooth
```

如果要用蓝牙耳机，还需要安装pulseaudio-bluetooth

```
sudo pacman -S pulseaudio-bluetooth
```

###### 2.Linux字体设置

前提：由于archlinux显示中文不太好看，所以对其进行改造。

(1).字体推荐

中文字体推荐使用：文泉驿、思源字体。安装如下：

```shell
sudo pacman -S wqy-microhei wqy-bitmapfont wqy-zenhei wqy-microhei-lite
sudo pacman -S adobe-source-han-sans-cn-fonts adobe-source-han-serif-cn-fonts
```

西文字体推荐使用dejavu、noto字体。

```shell
sudo pacman -S ttf-dejavu
sudo pacman -S noto-fonts noto-fonts-extra noto-fonts-emoji noto-fonts-cjk
```

(2).安装Windows Fonts字体和PingFang SC字体，字体在仓库里请看需求自行下载。

(3).将sans-serif、serif使用苹果苹方字体(PingFang SC)，Monospace使用WenQuanYi Micro Hei Mono，过程如下：[字体区别可以看这里](https://www.cnblogs.com/i0air/archive/2012/11/29/2794003.html)

```
mkdir ~/.config/fontconfig
vim ~/.config/fontconfig/fonts.conf
```

在fonts.conf文件中，写入如下内容：（//及后面字为说明用，正式使用请删除）

```
<?xml version='1.0'?>
<!DOCTYPE fontconfig SYSTEM 'fonts.dtd'>
<fontconfig>
 <alias>
  <family>sans-serif</family>        //注意：把显示的字体放在第一位就可以，后面做补充。
  <prefer>
   <family>PingFang SC</family>
   <family>WenQuanYi Micro Hei</family>
   <family>WenQuanYi Zen Hei</family>
   <family>WenQuanYi Zen Hei Sharp</family>
  </prefer>
 </alias>
 <alias>
  <family>serif</family>			//注意：把显示的字体放在第一位就可以，后面做补充。
  <prefer>
   <family>PingFang SC</family>
   <family>WenQuanYi Micro Hei Lite</family>
  </prefer>
 </alias>
 <alias>
  <family>monospace</family>		//注意：把显示的字体放在第一位就可以，后面做补充。
  <prefer>
   <family>WenQuanYi Micro Hei Mono</family>
   <family>WenQuanYi Zen Hei Mono</family>
   <family>PingFang SC</family>
  </prefer>
 </alias>
 <dir>~/.fonts</dir>
</fontconfig>
```

###### 3.deepin-wechat问题与解决

(1)无法输入中文的问题

- 打开终端cd 进入deepin-wine的运行脚本目录

```
sudo cd /opt/deepinwine/tools/
sudo chmod 777 run.sh  #修改权限
sudo vim run.sh   #编辑脚本
```

加入以下内容，保存并退出：

```
export GTK_IM_MODULE="fcitx"
export QT_IM_MODULE="fcitx" 
export XMODIFIERS="@im=fcitx"
```

如图:

![image11](https://github.com/zjkhy94/Archlinux-Manjaro-zjkhy94/blob/main/images/image11.png)

(2)修改deepin-wechat字体

前提：安装完微信后字体发虚、模糊，于是选择替换字体，要替换的字体为PingFang SC字体。

由于安装的是Spark打包的WeChat，所以需要输入下列命令：

```
WINEPREFIX=~/.deepinwine/Spark-WeChat deepin-wine5 winecfg
```

![image12](https://github.com/zjkhy94/Archlinux-Manjaro-zjkhy94/blob/main/images/image12.png)

打开“显示”，可知用的是“Tahoma”字体。

到/home/***/.deepinwine/Spark-WeChat目录下找到system.reg文件，用wps或者其他软件打开，然后搜索“Tahoma”，下面是修改成“PingFang SC”

![image13](https://github.com/zjkhy94/Archlinux-Manjaro-zjkhy94/blob/main/images/image13.png)

然后到deepin-wechat注册表修改。

首先输入下面命令：

```
WINEPREFIX=~/.deepinwine/Spark-WeChat deepin-wine5 regedit
```

然后将位置定位到HKEY_CURRENT_USER/Software/Wine/Fonts/Replacements

修改如图位置的字体：

![image14](https://github.com/zjkhy94/Archlinux-Manjaro-zjkhy94/blob/main/images/image14.png)![image15](https://github.com/zjkhy94/Archlinux-Manjaro-zjkhy94/blob/main/images/image15.png)

最后重新启动微信。
