---
title: "2023.7 Archlinux 安装教程"
date: 2023-07-05T11:31:00+08:00
tags:
  - Archlinux
---
### 一 基础安装
#### 1.禁用reflector
```shell
systemctl stop reflector.service
```
#### 2.确认是否为UEFI模式
```shell
ls /sys/firmware/efi/efivars
```
如果有输出则为UEFI模式
#### 3.更新系统时钟
```shell
timedatectl set-ntp true # 将系统时间与网络时间进行同步
timedatectl status # 检查服务状态
```
#### 4.更新镜像源
```shell
vim /etc/pacman.d/mirrorlist
```
在最上面添加
```shell
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch # 清华大学开源软件镜像站
```
#### 5.分区与格式化
转换磁盘为 gpt 类型
```shell
lsblk                       #显示分区情况 找到你想安装的磁盘名称
parted /dev/nvmexn1             #执行parted，进入交互式命令行，进行磁盘类型变更
(parted)mktable             #输入mktable
New disk label type? gpt    #输入gpt 将磁盘类型转换为gpt 如磁盘有数据会警告，输入yes即可
quit                        #最后quit退出parted命令行交互
```
EFI分区: /efi 1G  
Swap分区: 32G  
/分区：剩余
```shell
cfdisk /dev/nvmexn1 # 对安装 archlinux 的磁盘分区
```
查看分区
```shell
fdisk -l
```
格式化
```shell
mkswap /dev/nvmexn1pn
mkfs.ext4 /dev/nvmexn1pn
mkfs.vfat  /dev/nvmexn1pn
```
#### 6.挂载
```shell
mount /dev/nvmexn1pn /mnt
mkdir /mnt/efi
mount /dev/nvmexn1pn /mnt/efi
swapon /dev/nvmexn1pn
df -h
free -h
```
#### 7.安装系统
```shell
pacstrap /mnt base base-devel linux linux-firmware btrfs-progs
pacstrap /mnt dhcpcd iwd vim bash-completion
```
#### 8.生成fstab
```shell
genfstab -U /mnt >> /mnt/etc/fstab
cat /mnt/etc/fsta
```
#### 9.chroot
```shell
arch-chroot /mnt
```
#### 10.设置时区
```shell
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
hwclock --systohc
```
#### 11.本地化
```shell
vim /etc/locale.gen
```
反注释 en_US.UTF-8 zh_CN.UTF-8
```shell
locale-gen
echo 'LANG=en_US.UTF-8'  > /etc/locale.conf
```
#### 12.设置主机名
```shell
vim /etc/hostname
```
```shell
vim /etc/hosts
```
```shell
127.0.0.1   localhost
::1         localhost
127.0.1.1   arch
```
#### 13.设置root密码
```shell
passwd root
```
#### 14.安装微码
```shell
pacman -S intel-ucode # Intel
pacman -S amd-ucode # AMD
```
#### 15.安装引导
```shell
pacman -S grub efibootmgr os-prober ntfs-3g
grub-install --target=x86_64-efi --efi-directory=/efi --bootloader-id=GRUB
```
优化 grub
```shell
vim /etc/default/grub
```
去掉 GRUB_CMDLINE_LINUX_DEFAULT 一行中最后的 quiet 参数  
把 loglevel 的数值从 3 改成 5。这样是为了后续如果出现系统错误，方便排错  
加入 nowatchdog 参数，这可以显著提高开关机速度
```shell
GRUB_CMDLINE_LINUX_DEFAULT="loglevel=5 nowatchdog"
```

为了引导多系统在最后一行加上
```shell
GRUB_DISABLE_OS_PROBER=false
```
生成 grub
```shell
grub-mkconfig -o /boot/grub/grub.cfg
```
#### 16.完成安装
```shell
exit
umount -R /mnt
reboot
```
重启后
```shell
sytemctl start dhcpcd
sytemctl enable dhcpcd
```
### 二.进阶安装
#### 1.更新系统
```shell
pacman -Syu # 升级系统中全部包
```
#### 2.新建普通用户
```shell
useradd -m -G wheel -s /bin/bash insua
```
```shell
useradd -m -G wheel -s /bin/bash insua
```
首页sudo权限
```shell
EDITOR=vim visudo # 这里需要显式的指定编辑器，因为上面的环境变量还未生效
```
反注释
```shell
#%wheel ALL=(ALL:ALL) ALL
```
刷新 pacman
```shell
pacman -Syyu
```
### 三.安装桌面
#### 1.装x
```shell
pacman -S xorg-server xorg-xinit
```
#### 2.安装xfce4
```shell
pacman -S xfce4 xfce4-goodies
```
启动 xfce4
```shell
startxfce4
```
#### 3.安装lightdm
```shell
pacman -S lightdm lightdm-gtk-greeter
```
设定 greeter
```shell
/etc/lightdm/lightdm.conf
[Seat:*]
...
greeter-session=lightdm-gtk-greeter
```
启动 lightdm
```shell
systemctl start lightdm
```
#### 4.安装i3
```shell
pacman -S i3
```
#### 5.lightdm自动登录
```shell
groupadd autologin
gpasswd -a insua autologin
```
编辑配置文件
```shell
/etc/lightdm/lightdm.conf
[Seat:*]
autologin-user=insua
autologin-session=i3
```
可以正常登录后  
编辑环境变量
```shell
/etc/locale.conf
LANG=zh_CN.UTF-8
```
自动启动lightdm
```shell
systemctl enable lightdm
```
安装常用软件
```shell
pacman -S noto-fonts-cjk noto-fonts-emoji ttf-dejavu wqy-zenhei wqy-microhei file-roller unrar unace unzip p7zip
```
