--- title: 我需要的 vim 技巧
layout: post
author: WenfengShi
category: 私库
tags: [vim]
---

## 命令部分

### 分屏 Ctrl+W
- 启动窗口：`vim O2 filename1 filename2` 垂直分屏打开两个文件; `vim o3 filename1 filename2 filename3` 水平分屏打开三个文件
- 关闭窗口：`Ctrl+W c` 关闭当前窗口，如果是最后一个了，则不关闭；  `Ctrl+W q` 如果是最后一个，则直接关闭vim
- 屏间移动光标：`Ctrl+W`后再按`h/j/k/l`； `Ctrl+W w`
- 移动分屏位置：`Ctrl+W`后再按`H/J/K/L`
- 修改分屏尺寸：`Ctrl+W`后再按`=/+/-` 设置全部一样的高度/增加高度/减少高度；`Ctrl+W`后再按`>/<`设置增加/减少宽度
- 分屏转化为标签： `CTRL+W t`

### 多标签 tab
- 打开多个文件（多标签）:`vim -p file1 file2`; `:tabe file1`

### 窗口调节

- 设置vim默认打开窗口最大化的方式: windows下在*vimrc*中添加`au GUIEnter * simalt ~x`; linux下*vimrc*添加如下代码([其他详细配置参考链接](http://vim.wikia.com/wiki/Maximize_or_set_initial_window_size))

```vim
    if has("gui_running")
        set lines=999 columns=999
    else
        if esixts("+lines")
            set lines=50
        endif
    endif
```

### 专家命令

- 在新分屏中打开本文档所在的目录：`:H!`

### 有趣的命令

vim中输入`:h!` `:h 42`
