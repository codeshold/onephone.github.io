---
title: Shell Shortcuts（和Tab键一样实用）
layout: post
author: WenfengShi
category: 技术
tags: [shell]
---

> 总结并列举了15个有用的shell快捷键（基本都和Tab键一样实用），能有效的**加快**命令输入速度，只要记住了就基本会去用的键！

这是示意图（有助于理解，以避免歧义）：

![命令行示意图](http://static.zybuluo.com/wuzhimang/tu76v9smtew7puturdufptpz/image_1ask8c2au14ks1fb49tqq3l1qpj9.png)

## 快捷键
1. `Ctrl + A`：将光标移到**行首**
2. `Ctrl + E`：将光标移到**行尾**
14. `Alt + F`：将光标**向前**移动一个word
15. `Alt + B`：将光标**向后**移动一个word
4. `Ctrl + K`：删除光标**前面**的所有内容（即一直删到行尾）
3. `Ctrl + U`：删除光标**后面**的所有内容（若光标在行尾，则删除整行）
5. `Ctrl + W`：删除光标**后面**的一个字（word）
6. `Ctrl + L`：清空屏幕（相当于`clear`命令）--> **这个基本都知道**
7. `Ctrl + H`：和退格键的功能一样（Backspace）
8. `Ctrl + R`：搜索最近使用过的命令（**避免重复输入命令**）
9. `Ctrl + C`：kill掉正在运行的程序 -->  **这个应该都知道**
10. `Ctrl + D`：退出当前shell（**不用再输入exit/logout了**）
11. `Ctrl + Z`：将正在运行的程序挂起，通过`fg`命令可重新运行 --> **这个也应该都知道了**
13. `Esc + F`：*将光标前面的两个**word**的位置交换 --> 这个自己用的较少*
12. `Ctrl + T`：*将光标前面的两个**字符**的位置交换 --> 这个自己用的也少*

## 说明

> Shell快捷键很多，上面列举的15个大部分是自己平时经常用的，**git shell**，**bash shell**等都兼容！  
至于**CMD.EXE**，这个只能呵呵了...

## 参考 
1. [Wikipedia_Table_of_keyboard_shortcuts](https://en.wikipedia.org/wiki/Table_of_keyboard_shortcuts)：维基百科最全  
2. [Keyboard Shortcuts for Bash](https://www.howtogeek.com/howto/ubuntu/keyboard-shortcuts-for-bash-command-shell-for-ubuntu-debian-suse-redhat-linux-etc/)：一个比较好的geek网站
