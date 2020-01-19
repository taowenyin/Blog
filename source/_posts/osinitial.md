---
title: Linux系统装机工具和方法
mathjax: true
date: 2019-11-21 20:18:31
updated: {{ date }}
top: true
tags:
categories: [资源]
---

# Linux常用软件安装

## VIM

```bash
sudo add-apt-repository ppa:jonathonf/vim
sudo apt-get update
sudo apt-get install vim
```

## Shadowsocks-Qt5

```bash
sudo add-apt-repository ppa:hzwhuang/ss-qt5
sudo apt-get update
sudo apt-get install shadowsocks-qt5
```

## LibreOffice

```bash
sudo add-apt-repository ppa:libreoffice/ppa
sudo apt-get update
sudo apt-get install libreoffice
```

## Shutter

```bash
sudo add-apt-repository ppa:ubuntuhandbook1/shutter
sudo apt-get update
sudo apt-get install shutter
```

## VSCode

```bash
sudo apt-get install curl
curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main" > /etc/apt/sources.list.d/vscode.list'
sudo apt-get install apt-transport-https
sudo apt-get update
sudo apt-get install code
```

## Deluge

```bash
sudo add-apt-repository ppa:deluge-team/ppa
sudo apt-get update
sudo apt-get install deluge
```

## Emacs

```bash
sudo add-apt-repository ppa:kelleyk/emacs
sudo apt update
sudo apt install emacs25
```

## GIMP

```bash
sudo add-apt-repository ppa:otto-kesselgulasch/gimp
sudo apt-get update
sudo apt install gimp
```

## SimpleScreenRecorder

```bash
sudo add-apt-repository ppa:maarten-baert/simplescreenrecorder
sudo apt-get update
sudo apt-get install simplescreenrecorder
```

## Timeshift

```bash
sudo apt-add-repository -y ppa:teejee2008/ppa
sudo apt-get update
sudo apt-get install timeshift
```

## Sysmonitor

```bash
sudo add-apt-repository ppa:fossfreedom/indicator-sysmonitor
sudo apt-get update
sudo apt-get install indicator-sysmonitor
```

## VLC

```bash
sudo add-apt-repository ppa:videolan/master-daily
sudo apt-get update
sudo apt-get install vlc
```

## 有道字典

首先到有道词典官网下载deb安装包，注意有道词典Ubuntu版本只支持到Ubuntu 14.04,如果在Ubuntu 16.04上安装会失败，因为官方的Ubuntu版本的deb包依赖gstreamer0.10-plugins-ugly，但是该软件在16.04里面已经没有了。所以我们要下载64位的deepin版的安装包。经过测试，64位的deepin版的deb包在Ubuntu 16.04上安装成功。

## Guake

```bash
sudo add-apt-repository ppa:webupd8team/unstable
sudo apt-get update
sudo apt-get install guake
```

让Guake随系统自动启动，定位到"系统->首选项->启动程序->打开'启动程序喜好'程序"，单击'添加'按钮，在弹出的窗口中填写程序名称、命令和注释，命令一定不要写错（可以是程序名字guake，也可以是命令的绝对路径如/usr/bin/guake，建议采用前者），其他可以随便填。

## WPS

```bash
sudo apt-get install lib32ncurses5 lib32z1 libpng12-0
```

[下载最新的WPS地址](http://wps-community.org/downloads?vl=fonts#download)

> * 下载该字体，解压后将整个wps_symbol_fonts中的内容目录拷贝到 /usr/share/fonts/wps-office 目录下 
> * 进入字体目录 /usr/share/fonts/wps-office
> * sudo mkfontdir 
> * sudo mkfontscale 
> * fc-cache

## 指纹登录

```bash
sudo add-apt-repository ppa:fingerprint/fprint
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install libfprint0 fprint-demo libpam-fprintd
```

## UGet

```bash
sudo add-apt-repository ppa:plushuang-tw/uget-stable
sudo apt-get update
sudo apt-get install uget aria2
```

## uGetChromeWrapper

```bash
sudo add-apt-repository ppa:slgobinath/uget-chrome-wrapper
sudo apt-get update
sudo apt-get install uget aria2
```

## 远程桌面Remmina（L2W）

```bash
sudo apt-add-repository ppa:remmina-ppa-team/remmina-next
sudo apt-get update
sudo apt-get install remmina remmina-plugin-rdp libfreerdp-plugins-standard
```

## 远程桌面XFCE（W2L）

```bash
sudo apt-get install xrdp vnc4server xfce4
echo "xfce4-session" >~/.xsession
sudo service xrdp restart
```

## 主题

```bash
sudo apt-get install unity-tweak-tool

sudo add-apt-repository ppa:noobslab/themes
sudo apt-get update
sudo apt-get install flatabulous-theme

sudo add-apt-repository ppa:noobslab/icons
sudo apt-get update
sudo apt-get install ultra-flat-icons
```

## 蓝牙耳机

```bash
sudo apt-get install blueman bluez*
```

## VirtualBox

[下载地址](https://www.virtualbox.org/wiki/Linux_Downloads)

> * Qt 5.3.2 or later. Qt 5.6.2 or later is recommended.
> * SDL 1.2.7 or later. This graphics library is typically called libsdl or similar.

```bash
sudo dpkg -i virtualbox-6.0_6.0.4-128413~Ubuntu~xenial_amd64.deb
```

## Putty

```bash
sudo apt-get install libgtk2.0-dev
cd unix
./configure
make -f Makefile.gtk
cd ..
make install
```

## FTP的安装与设置

```bash
sudo apt-get install vsftpd
```

修改FTP配置文件/etc/vsftpd.conf

```bash
# 是否开放本地用户写权限，即是否允许上传
write_enable=YES

# 为YES时，禁止所有用户访问上级目录，只能访问各自的家目录
chroot_local_user=NO

# 设置vsftpd使用utf8编码的文件系统
utf8_filesystem=YES

# 如果不设置这个，会出现425 Security: Bad IP connecting.类似于这种的错误
pasv_enable=YES
pasv_min_port=40000
pasv_max_port=40010
pasv_promiscuous=YES
```

## Putty配色

```bash
* Default Foreground: 255/255/255  
* Default Background: 51/51/51  
* ANSI Black: 77/77/77  
* ANSI Green: 152/251/152  
* ANSI Yellow: 240/230/140  
* ANSI Blue: 205/133/63  
* ANSI Blue Bold 135/206/235  
* ANSI Magenta: 255/222/173 or 205/92/92  
* ANSI Cyan: 255/160/160  
* ANSI Cyan Bold: 255/215/0  
* ANSI White: 245/222/179 
```

## Telnet安装

```bash
sudo apt-get install openbsd-inetd
sudo apt-get install telnetd
sudo service openbsd-inetd restart
```

## FireFox安装

```bash
sudo add-apt-repository ppa:mozillateam/firefox-next 
sudo apt-get update
sudo apt-get install firefox
```

## Ubuntu下罗技优联管理关键

```base
sudo apt-get install solaar-gnome3
```

## 安装Ubuntu_Live镜像制作软件cubic

```bash
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 081525E2B4F1283B
sudo apt-add-repository ppa:cubic-wizard/release
sudo apt update
sudo apt install cubic
```

## 安装Tmux

```bash
sudo apt-get install tmux
```

## 安装SSH服务

```bash
sudo apt-get install openssh-server
```

## Peek

```bash
sudo add-apt-repository ppa:peek-developers/stable
sudo apt update
sudo apt install peek
```

## KCPTUN

```bash
#! /bin/sh
### BEGIN INIT INFO
# Provides:          kcptun
# Required-Start:    
# Required-Stop:
# Should-Start:      
# Default-Start:     1 2 3 4 5
# Default-Stop:
# Short-Description: kcptun
# Description:       Network file systems are mounted by
#                    /etc/network/if-up.d/mountnfs in the background
#                    when interfaces are brought up; this script waits
#                    for them to be mounted before carrying on.
### END INIT INFO

/opt/kcptun-linux-amd64/client_linux_amd64 -l :3000 -r 45.77.188.138:29900 -key "123456" -crypt none -nocomp -datashard 70 -parityshard 30 -mtu 1400 -sndwnd 2048 -rcvwnd 2048 -dscp 46 -quiet -mode fast2
```

## APT-FAST

```bash
sudo add-apt-repository ppa:apt-fast/stable
sudo apt-get update
sudo apt-get install apt-fast
```

## KVM

```bash
sudo apt-get install qemu-kvm qemu virt-manager virt-viewer libvirt-bin bridge-utils
```

## SQLiteBrowser

```bash
sudo add-apt-repository -y ppa:linuxgndu/sqlitebrowser
sudo apt-get update
sudo apt-get install sqlitebrowser
```

## MasterPDF

[官网](https://code-industry.net/downloads/)

## Texlive

[清华大学镜像下载地址](https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/Images/)

> * 1、图形化安装

```bash
sudo apt-get install perl-tk
sudo mount -o loop texlive2019.iso /mnt
cd /mnt/
sudo ./install-tl -gui
```

> * 2、设置字体

```bash
sudo cp /usr/local/texlive/2019/texmf-var/fonts/conf/texlive-fontconfig.conf /etc/fonts/conf.d/09-texlive.conf
sudo fc-cache -fsv
```

> * 3、设置环境变量

```bash
export MANPATH=${MANPATH}:/usr/local/texlive/2019/texmf-dist/doc/man
export INFOPATH=${INFOPATH}:/usr/local/texlive/2019/texmf-dist/doc/info
export PATH=${PATH}:/usr/local/texlive/2019/bin/x86_64-linux
```

> * 4、设置系统MAN

```bash
MANPATH_MAP /usr/local/texlive/2019/bin/x86_64-linux /usr/local/texlive/2019/texmf-dist/doc/man
```

> * 5、启动环境变量

```bash
source ~/.bashrc
source ~/.profile
```

> * 6、测试

```bash
tex --version
```

> * 7、弹出镜像

```bash
sudo umount /mnt
```

> * 8、VSCode配置

```json
"latex-workshop.latex.recipes": [
    {
        "name": "xelatex",
        "tools": [
            "xelatex"
        ]
    },
    {
        "name": "xelatex ➞ bibtex ➞ exlatex × 2",
        "tools": [
            "xelatex",
            "bibtex",
            "xelatex",
            "xelatex"
        ]
    }
],
"latex-workshop.latex.tools": [
    {
        "name": "xelatex",
        "command": "xelatex",
        "args": [
            "-synctex=1",
            "-interaction=nonstopmode",
            "-file-line-error",
            "%DOC%"
        ]
    },
    {
        "name": "bibtex",
        "command": "bibtex",
        "args": [
            "%DOCFILE%"
        ]
    }
],
```

> * 9、配置宏包的源

```bash
sudo tlmgr option repository https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/tlnet
sudo tlmgr update --self --all
```

## MiKTeX

```bash
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys D6BC243565B2087BC3F897C9277A7293F59E4889
echo "deb http://miktex.org/download/ubuntu xenial universe" | sudo tee /etc/apt/sources.list.d/miktex.list
sudo apt-get update
sudo apt-get install miktex
```

## Pinta

```bash
sudo apt-fast install pinta
```

# 资源下载 

## Android生成Key

[下载地址](https://pan.baidu.com/s/11KC0kjlk0rHle7SSY7l5RA)

提取码: pwkg

## 常用字体

[下载地址](https://pan.baidu.com/s/17htdYo6VgNKE6sjbywqKxQ)

提取码: jqs7

## Git Key

[下载地址](https://pan.baidu.com/s/1un69NhkHRY0Xxx5_qFisAw)

提取码: net4

## IntelliJIDEA认证服务

[下载地址](https://pan.baidu.com/s/1Mzqf2pCrSbI1bS5Manfy3Q)

提取码: e2ck

## OpenOCD

[下载地址](https://pan.baidu.com/s/1SZiGeIYuXT00cWc8I3rO1w)

提取码: gvsy

## SwitchyOmega

[下载地址](https://pan.baidu.com/s/1GmBNDaNxCNGBnhRzrINfUQ)

提取码: myjc

## Tomcat

[下载地址](https://pan.baidu.com/s/1VF4MOIaXNyNvVXtkIcEArg)

提取码: dtbt

## VirtualSerialPort

[下载地址](https://pan.baidu.com/s/1Uf7hZjXDGw9rVPGMNevVwQ)

提取码: db6k

## WPS缺失字体

[下载地址](https://pan.baidu.com/s/1ghDNODeuG7oO9Xb3tG8XSQ)

提取码: bjip

## KCPTUN配置

[下载地址](https://pan.baidu.com/s/1gV_sWFxGODWA2NioEReCVw)

提取码: xftw

# 系统配置

## 快捷方式

```bash
[Desktop Entry]
Name=Eclipse Java
Comment=Code Editing. Redefined.
GenericName=Text Editor
Exec=/opt/eclipse/java-oxygen/eclipse/eclipse
Icon=/opt/eclipse/java-oxygen/eclipse/icon.xpm
Type=Application
Categories=Utility;TextEditor;Development;IDE;
Keywords=java;
```

## 解决Windows与Ubuntu双系统下的时间问题

```bash
sudo apt-get install ntpdate
sudo ntpdate time.windows.com
sudo hwclock --localtime --systohc
```

重新进入Windows系统调整下时间就可以

## Ubuntu系统下安装最新的Nvidia驱动

> * 1、到Nvidia[官网下载最新.run驱动文件](https://www.nvidia.cn/Download/index.aspx?lang=cn)

> * 2、添加Nvidia最新PPA

```bash
sudo add-apt-repository ppa:graphics-drivers/ppa 
sudo apt update 
```

> * 3、在/etc/modprobe.d/中创建一个blacklist-nouveau.conf文件，并添加nouveau的禁用配置项

```bash
blacklist nouveau
blacklist lbm-nouveau
options nouveau modeset=0
alias nouveau off
alias lbm-nouveau off
```

> * 4、重启计算机在登录页面使用`Ctrl+Alt+F1`进入字符界面后，键入下面指令，如果没有任何信息，表示Nouveau驱动禁用成功

```bash
lsmod | grep nouveau
```

> * 5、使用.run文件删除当前所有的Nvidia驱动（不要手动删除，用自动方式更安全）

```base
sudo chmod +x NVIDIA-Linux-x86_64-xxx.run
sudo ./NVIDIA-Linux-x86_64-xxx.run –uninstall
```

> * 6、使用PPA指令进行安装，并重启计算机，就可以在**设置->详细信息**中看到

```base
sudo apt-get install nvidia-xxx
sudo reboot
```

> * 7、在**设置->详细信息**中检查是否安装成功，也可以使用下面指令进行查看

```base
nvidia-smi
```

## 设置应用程序开机自动启动

> * 1、复制/etc/init.d目录下的一个.sh文件，并进行修改

> * 2、执行下面的指令，把该启动文件添加到启动列表中

```base
sudo update-rc.d huadian.sh defaults
```

## 解决Ubuntu下ZIP文件乱码

```base
sudo apt-get install unar
unar XXX.zip
```

## Ubuntu下增加ttyUSB权限

> * 1、创建USB规则文件

```bash
sudo gedit /etc/udev/rules.d/70-ttyusb.rules
```

> * 2、添加文件内容

KERNEL=="ttyUSB[0-9]*", MODE="0666"

> * 3、修改USB驱动权限

```bash
sudo chmod 666 /dev/ttyUSB0
```

## 双系统GRUB配置

> * 在终端中输入：sudo gedit /etc/default/grub
> * 将文本”GRUB_DEFAULT=0“中的0改成win7系统的序号4
> * 在终端输入：sudo update-grub2

## Ubuntu下增加Keyboard权限

> * 1、创建Keyboard规则文件

```bash
sudo gedit /etc/udev/rules.d/70-usbkbd.rules
```

> * 2、添加文件内容

KERNEL=="event*", NAME="input/%k", MODE="0666"

> * 3、修改Keyboard驱动权限

```bash
sudo chmod 666 /dev/input/by-id/usb-Logitech_USB_Receiver-if02-event-kbd
```

## Ubuntu下libpython安装错误解决

Ubuntu更新“libpython3.6-stdlib_3.6.5-5~16.04.york1_amd64.deb”时错误导致无法更新

```bash
sudo dpkg --install --force all /var/cache/apt/archives/libpython3.6-stdlib_3.6.5-5~16.04.york1_amd64.deb

sudo apt install -f
```

## 解决内存占满导致死机的问题

编辑内存清空脚本

```bash
#!/bin/bash

sync && echo 1 | sudo tee /proc/sys/vm/drop_caches
sync && echo 2 | sudo tee /proc/sys/vm/drop_caches
sync && echo 3 | sudo tee /proc/sys/vm/drop_caches
```

利用cron创建定时服务

```bash
sudo -s

crontab -e
```

添加如下内容

> 0 * * * * /opt/clearBuffer.sh

## 设置树莓派屏幕分辨率

config.txt设置

> hdmi_group=2

> hdmi_mode=87

> hdmi_cvt 800 480 60 6 0 0 0

# 开发工具

## Java

```bash
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer
sudo apt-get install oracle-java8-set-default
```

## Apache2

```bash
sudo add-apt-repository ppa:ondrej/apache2
sudo apt-get update
sudo apt install apache2
```

## PHP

```bash
sudo add-apt-repository ppa:ondrej/php
sudo apt-get update
sudo apt-get install php7.1 php7.1-dev libapache2-mod-php7.1 libevent-dev php7.1-bz2 php7.1-cgi php7.1-cli php7.1-common php7.1-curl php7.1-gd php7.1-fpm php7.1-imap php7.1-json php7.1-mbstring php7.1-mcrypt php7.1-mysql php7.1-odbc php7.1-snmp php7.1-soap php7.1-sybase php7.1-tidy php7.1-xml php7.1-xmlrpc php7.1-xsl php7.1-zip php-pear
sudo pecl install event
sudo pecl install xdebug
```

> 修改WebServer位置

```bash
/etc/apache2/sites-enabled/000-default.conf

DocumentRoot /home/taowenyin/MyCode/WebServer

<Directory /home/taowenyin/MyCode/WebServer>
    Options FollowSymLinks Indexes
    Require all granted
</Directory>
```

## Git

```bash
sudo add-apt-repository ppa:git-core/ppa
sudo apt-get update
sudo apt install git
```

## Gitkraken

## Node.js

```bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
nvm install node
# 切换到国内镜像
npm config set registry https://registry.npm.taobao.org
```

## Python的安装与配置

```bash
# sudo add-apt-repository ppa:jonathonf/python-2.7
# sudo add-apt-repository ppa:jonathonf/python-3.7

sudo add-apt-repository ppa:deadsnakes/ppa

sudo apt-get update

sudo apt-get install python3.7
sudo apt-get install python2.7

# 把Python2、3加入update-alternatives选择列表，最后1表示优先级，数字越大优先级越高
sudo update-alternatives --install /usr/bin/python python /usr/bin/python2.7 1
sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.7 2

# 选择需要的默认Python版本
sudo update-alternatives --config python

# 修改Python3.5和3.7加入update-alternatives选择列表，最后1表示优先级，数字越大优先级越高
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.7 1
sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.5 2

# 选择需要的默认Python3版本
sudo update-alternatives --config python3
```

## Android环境变量

```bash
export ANDROID_HOME=/home/taowenyin/Android/Sdk
export ANDROID_SDK_HOME=/home/taowenyin/Android
export PATH=$PATH:$ANDROID_HOME/platform-tools:$ANDROID_HOME/tools
```

## C++和32bit库

```bash
sudo apt-get install lib32ncurses5 lib32z1 libgl1-mesa-dev ttf-wqy-*
```

## IntelliJIDEALicenseServer

[下载地址和使用说明](http://blog.lanyus.com/)


> * 在终端中输入并添加：sudo gedit /etc/rc.loacl

```bash
/opt/IntelliJIDEALicenseServer/IntelliJIDEALicenseServer_linux_amd64 -p 1017 -u taowenyin
```

## Tomcat9安装过程

> * 1、下载Tomcat，并移动到opt目录下

```bash
wget http://mirrors.shu.edu.cn/apache/tomcat/tomcat-9/v9.0.13/bin/apache-tomcat-9.0.13.tar.gz
tar -xzf apache-tomcat-9.0.13.tar.gz
sudo chmod 777 /opt/ -R
sudo mv apache-tomcat-9.0.13 /opt/tomcat
sudo nano /opt/tomcat/conf/tomcat-users.xml
```
> * 2、编辑Tomcat管理用户，及管理权限

```bash
sudo nano /opt/tomcat9/conf/tomcat-users.xml
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<tomcat-users xmlns="http://tomcat.apache.org/xml"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://tomcat.apache.org/xml tomcat-users.xsd"
              version="1.0">
    <!-- 设置可以访问管理 -->
	<role rolename="manager"/>
    <!-- 设置可以访问管理员 -->
	<role rolename="admin"/>
    <!-- 设置可以访问管理页面 -->
	<role rolename="manager-gui"/>
    <!-- 设置可以访问管理员页面 -->
	<role rolename="admin-gui"/>
    <!-- 设置可以访问管理页面和管理员页面的账户 -->
	<user username="admin" password="admin" roles="manager-gui,admin-gui,manager-script"/>
</tomcat-users>
```

> * 3、创建系统级的服务访问

```bash
sudo nano /etc/systemd/system/tomcat.service
```

```apache
[Unit]
Description=Apache Tomcat Web Application Container
After=network.target

[Service]
Type=forking

Environment=JAVA_HOME=/usr/lib/jvm/java-8-oracle
Environment=CATALINA_PID=/opt/tomcat/temp/tomcat.pid
Environment=CATALINA_HOME=/opt/tomcat
Environment=CATALINA_BASE=/opt/tomcat
Environment='CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC'
Environment='JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom'

ExecStart=/opt/tomcat/bin/startup.sh
ExecStop=/opt/tomcat/bin/shutdown.sh

User=[登录账户]
Group=[登录账户组]
UMask=0007
RestartSec=10
Restart=always

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl daemon-reload
sudo systemctl start tomcat.service
sudo systemctl restart tomcat.service
sudo systemctl enable tomcat.service
```

> * 4、修改webapps/manager和webapps/host-manager下的context.xml为Tomcat下的各应用添加远程访问

```xml
<Context antiResourceLocking="false" privileged="true" >
  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="\d+\.\d+\.\d+\.\d+" />
  <Manager sessionAttributeValueClassNameFilter="java\.lang\.(?:Boolean|Integer|Long|Number|String)|org\.apache\.catalina\.filters\.CsrfPreventionFilter\$LruCache(?:\$1)?|java\.util\.(?:Linked)?HashMap"/>
</Context>
```

## MySQL5.7远程登录设置

安装MySQL

```bash
wget https://dev.mysql.com/get/mysql-apt-config_0.8.11-1_all.deb
sudo dpkg -i mysql-apt-config_0.8.11-1_all.deb
sudo apt-get update
sudo apt-get -y install mysql-server
```

编辑MySQL配置文件

```bash
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
```

在下面行的开头加上#，注释掉该行，然后保存退出：

```bash
bind-address = 127.0.0.1
```

登录MySQL，执行修改权限指令，并重启服务器：

```bash
mysql -u root -p
grant all privileges on *.* to root@"%" identified by "[root账户密码]" with grant option;
flush privileges;
exit;
service mysql restart
```

## 为Angular开启Apache的Rewrite功能

> * 1、在网站根目录下创建文件.htaccess，并填入如下内容

```bash
RewriteEngine On
    # If an existing asset or directory is requested go to it as it is
    RewriteCond %{DOCUMENT_ROOT}%{REQUEST_URI} -f [OR]
    RewriteCond %{DOCUMENT_ROOT}%{REQUEST_URI} -d
    RewriteRule ^ - [L]
    # If the requested resource doesn't exist, use index.html
RewriteRule ^ /index.html
```

> * 2、开启Apache的Rewrite功能

```bash
sudo a2enmod rewrite
```

> * 3、修改apache2.conf下所有Directory的AllowOverride设置为All

```bash
<Directory />
     Options Indexes FollowSymLinks
     AllowOverride All
     Require all denied
</Directory>
```

> * 4、修改/etc/apache2/sites-available下的000-default.conf中的DocumentRoot为/var/www/html/dist/ElectricIOTWeb

## 解决LabView的Ni_Application_Web_Server端口与Tomcat冲突的问题（8080）

进入下面的网站，点击Web服务器配置，在HTTP端口中修改相应端口号

```http
http://localhost:3582
```

## 解决LabView的mDNS_Responder_Service与IntelliJ_IDEA_Java_Web调试端口冲突的问题（1099）

在Debug设置里面修改JMX端口号，避开1099

## 查看端口被占用的情况

```base
# 获取PID号
netstat -aon|findstr "端口号"

# 根据PID号查询相应的进程
tasklist|findstr "PID号"
```

## gcc-arm-none-eabi和OpenOCD安装

```base
sudo add-apt-repository ppa:team-gcc-arm-embedded/ppa
sudo apt-get update
sudo apt-get install gcc-arm-none-eabi
sudo apt-get install openocd
```

## CLion安装STM32开发环境

[参考链接](https://me.csdn.net/PoJiaA123)

> * 1、下载STM32CubeMx（注：Ubuntu 16.04只能使用V4.27.0）和CLion软件。

> * 2、安装gcc-arm-none-eabi和OpenOCD。

> * 3、在CLion中安装“OpenOCD + STM32CubeMX support for ARM embedded development”插件。

> * 4、在CLion的Build标签下的OpenOCD选项中设置OpenOCD目录（注：Ubuntu中使用命令行安装的OpenOCD不需要设置）。

> * 5、使用STM32CubeMx生成SW4STM32项目，但不要打开项目。

> * 6、使用CLion打开并选择项目（注：一切默认），并在工具栏Tool中现在“Update CMake project with STM32CubeMx project”。

> * 7、期间会跳出“Board config file”，选择一个和自己MCU相同的板子。

**改为C++编程**

> * 8、修改main.c为main.cpp。

> * 9、修改CMakeLists.txt中的PROJECT(HelloSTM32ROS C CXX ASM)为PROJECT(HelloSTM32ROS CXX C ASM)（注：默认实际已经支持C++）。

## CLion使用OpenOCD进行调试STM32F103

> * 1、在CLion中选择Board Config File为stm3210b_eval.cfg。

> * 2、在终端中执行下面的指令，使得OpenOCD与STM32链接。

```base
openocd -f /usr/share/openocd/scripts/interface/stlink-v2.cfg -f /usr/share/openocd/scripts/target/stm32f1x.cfg
```

## Ubuntu下安装STLink

> * 1、准备工作。

```bash
sudo apt-get install libusb-1.0-0
sudo apt-get install libgtk-3-dev 
```

> * 2、下载并安装STLink

```bash
git clone https://github.com/texane/stlink.git
cd stlink/
make release 
make debug 
cd build/
cmake -DCMAKE_BUILD_TYPE=Debug ..
make
cd Release/
sudo make install
sudo ldconfig
```

## 解决STM32CubeMX生成的代码不能用OpenOCD和ST-Link调试

**原因：** 由于STM32CubeMX默认生成的代码没有开启调试，使得OpenOCD、Keil等软件都不能进行仿真

> * 1、在SYS的Debug选项中选择“Trace Asynchronous Sw“

> * 2、用烧录软件重新烧录（可能的方法，在烧录之前把Boot0和Boot1拉高，烧录完成后再拉低）

> * 3、就可以进行调试

## OpenCV(>=3.4.1)

```bash
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=RELEASE -DCMAKE_INSTALL_PREFIX=/usr/local -DINSTALL_PYTHON_EXAMPLES=ON -DINSTALL_C_EXAMPLES=ON -DOPENCV_EXTRA_MODULES_PATH=../../opencv_contrib-3.4.2/modules -DPYTHON3_EXECUTABLE=/usr/bin/python3.5 -DPYTHON3_INCLUDE_DIR=/usr/include/python3.5 -DPYTHON3_INCLUDE_DIR2=/usr/include/x86_64-linux-gnu/python3.5m -DPYTHON3_LIBRARY=/usr/lib/x86_64-linux-gnu/libpython3.5m.so -DPYTHON3_NUMPY_INCLUDE_DIRS=/usr/local/lib/python3.5/dist-packages/numpy/core/include/ -DPYTHON2_EXECUTABLE=/usr/bin/python3.5 -DPYTHON2_INCLUDE_DIR=/usr/include/python3.5 -DPYTHON2_INCLUDE_DIR2=/usr/include/x86_64-linux-gnu/python3.5m -DPYTHON2_LIBRARY=/usr/lib/x86_64-linux-gnu/libpython3.5m.so -DPYTHON2_NUMPY_INCLUDE_DIRS=/usr/local/lib/python3.5/dist-packages/numpy/core/include/ -DBUILD_EXAMPLES=ON ..
make;sudo make install
sudo gedit /etc/ld.so.conf.d/opencv.conf
```

> * 填入/usr/local/lib

```bash
sudo ldconfig
sudo gedit /etc/bash.bashrc
```

> * 填入PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig和export PKG_CONFIG_PATH

```bash
source /etc/bash.bashrc
sudo updatedb
```

## QT环境配置

```base
export QTDIR=/opt/Qt/Qt5.12.1/5.12.1/gcc_64
export PATH=$QTDIR/bin:$PATH
export LD_LIBRARY_PATH=$QTDIR/lib:$LD_LIBRARY_PATH
```

## 添加最新的Boost库

```bash
sudo add-apt-repository ppa:mhier/libboost-latest
sudo apt-get update
```

## 虚拟串口的使用

```bash
python3 VirtualSerialPort.py
```

## 设置VSCode自动格式化

> * 1、“Format On Save”设为true

> * 2、“Detect Indentation”设为false

## SW4STM32项目转化为C++

> * 1、右键项目，选择“Convert to C++”

> * 2、修改main.c为main.cpp

## 修改GitHub的SSH端口解决22端口超时问题

> * 1、在id_rsa和id_rsa.pub创建config文件

> * 2、写入如下内容

```bash
Host github.com
User wenyin.tao@163.com
Hostname ssh.github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa
Port 443
```

> * 3、修改GitHub端口

```bash
ssh -T git@github.com
```

# ROS开发记录

## rosserial_stm32的编译与安装

[参考链接1](https://blog.csdn.net/m0_38089090/article/details/79870815)

[参考链接2](https://blog.csdn.net/qq_37416258/article/details/84844051)

> * 1、安装ROS串口库，指令如下。

```bash
sudo apt-get install ros-kinetic-rosserial
```

> * 2、进入官网下载rosserial_stm32（[官网](http://wiki.ros.org/rosserial)）。

> * 3、把下载后的文件解压并重命名为rosserial_stm32，进入ROS的工作空间。

> * 4、复制rosserial_stm32文件夹到ROS工作空间的src目录中。

> * 5、在ROS工作空间中执行catkin_make指令进行编译。

> * 【可能需要的步骤】执行安装指令，并载入环境安装目录中的环境变量，指令如下。

```bash
catkin_make install
source ROS工作空间/install/setup.bash
```

> * 6、进入STM32CubeMx创建的项目目录，并执行如下指令。

```bash
rosrun rosserial_stm32 make_libraries.py .
```

> * 7、修改Inc目录下的STM32Hardware.h文件，并把头文件中的“stm32f3xx_halxxx.h”修改为“stm32f1xx_halxxx.h”。

> * 8、修改main.c->main.cpp。

> * 9、修改CMakeLists.txt中的PROJECT(HelloSTM32ROS C CXX ASM)为PROJECT(HelloSTM32ROS CXX C ASM)。

> * 10、修改Inc目录下的STM32Hardware.h文件中的定时器和串口对象。

```c
extern TIM_HandleTypeDef htim2;
extern UART_HandleTypeDef huart1;
```

> * 11、添加roscore文件夹和mainpp.cpp、mainpp.h，并完成相应内容。（注：文件名和文件夹名可以自定义）

> * 12、使用该库需要启动rosserial作为数据传递的中介。

```bash
rosrun rosserial_python serial_node.py
```

## ROS串口标准库serial的编译与使用

[参考链接](http://wjwwood.io/serial/)

> * 1、下载serial库

```bash
git clone https://github.com/wjwwood/serial.git
```

> * 2、编译serial库

```bash
sudo apt-get install doxygen
cd serial
make
make test
make docs
make install
make uninstall（卸载）
```

> * 3、复制到ros库

```bash
cp -R /tmp/usr/local/include/ /opt/ros/kinetic/
cp -R /tmp/usr/local/lib/ /opt/ros/kinetic/
cp -R /tmp/usr/local/share/ /opt/ros/kinetic/
```

> * 4、创建serial依赖的功能包

```bash
catkin_create_pkg <package_name> serial std_msgs roscpp rospy
```

## ROS机械臂控制问题集锦

[参考链接](https://blog.csdn.net/weixin_39579805)

## ROS节点调试

> * 1、将下面两行加入到CMakeLists.txt中

```bash
set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g")
set (CMAKE_VERBOSE_MAKEFILE ON)
```

> * 2、修改launch.json中的program

```bash
${workspaceFolder}/devel/lib/<package_name>/<node_name>
```

## ROS键盘检测

> * 1、进入官网下载keyboard_reader（[官网](https://github.com/UTNuclearRoboticsPublic/keyboard_reader)）。

> * 2、复制keyboard_reader文件夹到ROS工作空间的src目录中。

> * 3、修改keyboard_reader.cpp文件中的第118行内容。

> * 4、修改keyboard_reader.cpp文件中的第51行内容，以选择正确的键盘。

> * 5、在ROS工作空间中执行catkin_make指令进行编译。

> * 6、keyboard_reader项目的做法主要是创建一个“keyboard”话题，并发送自定义的Key.msg消息

> * 7、创建订阅。

> * 8、修改package.xml，添加keyboard_reader依赖。

```xml
<build_depend>keyboard_reader</build_depend>
<build_export_depend>keyboard_reader</build_export_depend>
<exec_depend>keyboard_reader</exec_depend>
```

> * 9、修改CMakeLists.txt，添加keyboard_reader依赖。

```makefile
find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  std_msgs
  keyboard_reader # 添加的内容
)

# 添加MSG依赖
add_dependencies(<package_name> <depend_package_name>_generate_messages_cpp)
add_dependencies(ros_keyboard keyboard_reader_generate_messages_cpp)
```

## Gazebo模型

```http
https://bitbucket.org/osrf/gazebo_models/downloads/
```

下载并解压缩后放到/home/taowenyin/.gazebo/models目录下