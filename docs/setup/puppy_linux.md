# Puppy Linux 安装使用说明

Puppy Linux 是一个迷你又很实用的 Linux 发行版。可以很方便地安装电脑和U盘上，对一些旧电脑也能很好地兼容。本安装指南基于 fossapup64 V9.5 （兼容Ubuntu 20.04）版本，这个版本支持 Ubuntu 20.04 的 deb 包。

访问[Puppy Linux 官网](https://puppylinux.com/)可以了解更多信息。

## 安装 Puppy Linux

下载 [fossapup64-9.5.iso](https://www.bilive.com/site_media/media/tools/fossapup64-9.5.iso) 安装 Puppy Linux fossapup64 V9.5。

## 安装汉化包

fossapup64 V9.5 缺省不带中文包，所以需要先安装汉化包。

下载 [fcitx64_4.2.9-en-zh-bionic-21.2.5.pet](https://www.bilive.com/site_media/media/tools/fcitx64_4.2.9-en-zh-bionic-21.2.5.pet) ，点击即可安装fcitx输入法和中文环境支持。

## 系统汉化

下载 [fossapup64-9.5.0-zhcn-21.0.0s.pet](https://www.bilive.com/site_media/media/tools/fossapup64-9.5.0-zhcn-21.0.0s.pet) ，点击即可安装系统汉化包，如果只想使用英文界面，可以跳过这一步，这一步并不是必须的。

## 安装 biLive 系列软件

- 需要使用 biForm 的话，点击 [biform_3.1.005.pet](https://www.bilive.com/site_media/media/tools/) 进行安装，[biformlzcx_3.1.005.pet](https://www.bilive.com/site_media/media/tools/biformlzcx_3.1.005.pet) 是龙众创芯专版 ，两个选一个就可以。

- 需要使用 biReader 的话，点击 [biReader_3.1.005.pet](https://www.bilive.com/site_media/media/tools/biReader_3.1.005.pet) 进行安装。

- 需要使用 智应软件中心 的话，点击 [dziapp_1.0.001.pet](https://www.bilive.com/site_media/media/tools/dziapp_1.0.001.pet) 进行安装，安装程序会将 .PFF/.PFP 文件和智应软件中心进行绑定，以后直接点击 .PFF/.PFP 文件就可以运行这些biLive跨平台通用桌面程序了。

## 设置输入法

修改 /etc/profile 在最后加入以下内容

'''shell
export GTK_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
export QT_IM_MODULE=fcitx
'''

保存后重启系统。这样上一步安装的程序就可以输入中文了。

## 注意事项

- 第一次启动puppy linux后关机或重启时，puppy linux会提示是否保存用户数据，如果以后要继续使用这些设置和安装好的软件，一定要选择“保存”，否则以上的安装和设置在关闭系统时都会丢失。

- 以上步骤都是基于使用 Puppy Linux 缺省的root账号进行的，如果使用其它账号，需要按情况灵活调整一下。
