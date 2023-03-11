---
title: "阿米洛在Arch下的键位映射"
date: 2023-03-11T10:49:57+08:00
tags:
- Keyboard
- Archlinux
---
阿米洛键盘的F区在 Archlinux 是有问题的，通过 Xmodmap 去修改键位映射  
下载 [Xmodmap](/file/2023/Xmodmap) 文件，修改为隐藏文件，放入 ~ 目录  
修改 **~/.xinitrc**，增加代码  
```shell
[[ -f ~/.Xmodmap ]] && xmodmap ~/.Xmodmap
```


