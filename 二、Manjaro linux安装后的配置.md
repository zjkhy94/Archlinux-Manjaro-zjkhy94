# 二、Manjaro linux安装后的配置

### （一）、系统更新

###### 1、切换镜像源

打开终端，执行下面命令：

```
sudo pacman-mirrors -i -c China -m rank
```

等待片刻，弹出对话框选择最快的镜像源。

然后更新系统：

```
sudo pacman -Syyu
```

###### 2、添加archlilnuxcn源（arch中文社区的软件源仓库）

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

到此系统更新完成。

### （二）、常用软件

Manjaro系统默认使用pacman作为软件包管理器，这里给出pacman的简单使用方法：

```
sudo pacman -S 软件名　# 安装
sudo pacman -R 软件名　# 删除单个软件包，保留其全部已经安装的依赖关系
sudo pacman -Rs 软件名 # 除指定软件包，及其所有没有被其他已安装软件包使用的依赖关系
sudo pacman -Ss 软件名  # 查找软件
sudo pacman -Sc # 清空并且下载新数据
sudo pacman -Syu　# 升级所有软件包
sudo pacman -Qs # 搜索已安装的包
```

###### 1、aurman

aurman 是最好的 AUR 助手之一，可以搜索 AUR、解决包依赖，在构建 AUR 包前检查 PKGBUILD 的内容等。

安装 aurman：

```
git clone https://aur.archlinux.org/aurman.git
cd aurman
makepkg -si
```

使用 aurman：

用名字搜索：

```
aurman -Ss <package-name>
```

安装：

```
aurman -S &lt;package-name>
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

###### 3、安装base-devel (安装基础包，避免安装其他包时出现依赖错误)

```
sudo pacman -S base-devel
```

###### 4、安装输入法 (搜狗输入法)

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

###### 5、安装chrome

```
yay -S google-chrome
```

###### 6、Markdown编辑器：Typora

```
sudo pacman -S typora
```

###### 7、安装百度网盘

```
yay -S baidunetdisk-bin
```

###### 8、安装wps

```
sudo pacman -S wps-office ttf-wps-fonts
```

###### 9、安装debtap

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

###### 10、安装todesk

方法一：

```
yay -S todesk
```

如果安装有问题可以选择下面方法：

方法二：

到todesk官网下载（Debian、Ubuntu（x64）版本）

![image2](https://github.com/zjkhy94/manjaro-linux-zjkhy94/blob/main/images/image2.png)

然后使用软件9的方法安装，例如：

```
(1)sudo debtap todesk.deb
(2)sudo pacman -U todesk.tar.xz
```
###### 11、安装网易云音乐

```
sudo pacman -S netease-cloud-music
```

###### 12、安装Foxit

```
yaourt foxit
```

###### 13、安装filezilla 

```
sudo pacman -S filezilla
```

###### 14、安装screenfetch (在终端里输出你的系统logo和状态)

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

效果如下：

![image3](https://github.com/zjkhy94/manjaro-linux-zjkhy94/blob/main/images/image3.png)

###### 15.安装Deepin-wechat、Deepin-TIM

```
yay -S com.qq.weixin.spark******(Deepin-wechat)
yay -S com.qq.tim.spark******(Deepin-tim)
```


