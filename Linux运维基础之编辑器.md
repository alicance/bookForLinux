- 1.1 初识Linux文本编辑器
- 1.2 Vim
   + 1.2.1 vim简介
   + 1.2.2 vim安装
   + 1.2.3 vim配置
   + 1.2.4 vim工作模式
   + 1.2.5 vim使用
   + 1.2.6 终端小技巧
- 2.2 Emacs
   + 2.2.1 vim简介
   + 2.2.2 vim安装
   + 22.3 vim配置
   + 2.2.4 vim使用
- Sed
- awk
___

**初识Linux文本编辑器 **
​    文本编辑器是计算机处理文本的一款应用软件，主要用于文件的查看与编辑。文本编辑器对于所有使用过计算机的人并不默认，可谓众所周知。无论在生活还是工作上，每个计算机用户离不开文本编辑器的依赖，编辑器的用处范围特别广，在计算机的领域范围无处不见，比如查看文档、修改配置文件、存储文本信息数据等。编辑器的典型功能有如下：
- 记录数据信息
- 文字排版
- 剪切、复制、粘贴
- 撤销和恢复
- 查找和替换 

​    众多的计算机系统都有编辑器，Windows下常见的有记事本，Mac下常见的有编辑器，Linux系统也不例外，Linux系统下有众多的文本编辑器，比较出众的有vim、emacs、nano、gedit、sed以及awk等。sed和awk更多地体验在终端，也就是以指令的方式来使用。
___

#### vim ####
**vim简介**
​	vim全称IMproved，它是一个基于终端、功能强大、易扩展、跨平台的一款编辑工具。说起vim编辑器，那么我们首先得认识一下vi编辑器，vi编辑器得全称是visual editor的意思，是Unix和Linux操作系统中的默认自带编辑器，就好比我们的Windows操作系统自带的记事本一样，属于纯文本编辑器。从vim的全称可以看的出来，vim是继承vi开发的，完全兼容vi编辑器，在此同时，vim编辑器还多非常多的增强功能，vim是在Uinx/Linux系统下公认最好的一款终端编辑器，在程序员的眼中使用vim编辑器是一件非常愉快的事情。

​	vim有一个与众不同的特别之处，那就是无需鼠标的介入，而仅仅依赖键盘的情况下，依然可以非常高效地进行编辑文件，既可以脱离鼠标，还可以高效编辑文件，这样也是一个不错的好处，想想，Linux在大多数系统都是拿来搭建服务器的，这样的系统并没有可视化的界面，都是使用终端字符界面的，要是和Linux服务器打交道的话，选择vim那就最好不过了，甚至在有界面的Linux计算机上也是。为何vim有如此强大的功能呢，我们来谈谈vim有哪些功能或特点？
- 几乎完全兼容vi
- 具有多缓冲功能
- 可以任意分区多个窗口编辑，包括横与竖
- 具备列表与字典功能的脚本语言
- 自动补全功能
- 可以多次撤销和重做
- 支持C/C++、PHP、Perl、Rubby、Python等多种语言的自动缩排
- 支持语法高亮
- 崩溃后文件可恢复
- 具备session功能，即光标位置和打开的缓冲状态的保存复原
- 可远程编辑文件
- 可分析两个文件对比差异
- 利用ctags的标签中跳转
- 无需依赖键盘即可轻松高效编辑文件编辑
- vim具有非常丰富的扩展组件

**vim安装**

  	在Unix/Linux下安装Vim是及其简单的一件事，只需要几条命令即可，这种安装方法属于Linux包管理安装！
基于Debian的Linux发行版的系统安装方式如下，比如Ubuntu系统：

```
$ apt-get update
$ apt-get install vim
# 安装过程省略...
```

基于Red Hat的Linux发行版的系统安装方式如下，比如CentOS系统：
```
$ yum update
$ yum install vim
# 安装过程省略...
```

基于pacman包管理器的Linux发行版安装方式如下，比如Arch Linux系统：
```
$ pacman -S pacman
$ pacman -S vim
# 安装过程省略...
```

安装完毕之后我们可以在终端通过`vim -v`命令来查看vim的安装版本。

![vim -v](http://upload-images.jianshu.io/upload_images/1678789-8608c80f3d0dc487.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

​	除了包管理的安装方法之外还可以下载已经打包好的二进制tar.gz文件直接解压使用，或者下载vim的源码在本地进行编译安装。

***vim配置**
​	既然已经安装完了vim，当然得研究其配置文件，在Ubuntu的系统下，vim的配置文件位于`/etc/vim/vimrc`路径下。在终端上执行`cat /etc/vim/vimrc`命令打印出vim配置文件的内容可以知道vim配置文件的规则：
1) 使用单双引号"标明注释
2) 使用set <配置字段> 开启某个功能
3) 自定义的用户配置文件可以通过source引入，很推荐！这样可以起到备份的作用

vim有哪配置参数呢？下面我们具体来说明vim原生的配置参数。
```
set nocompatible               " 不要使用vi的键盘模式，而使用vim自带的
set syntax=on                  " 开启语法高亮
set noeb or noerrorbells       " 关闭错误信息响铃
set confirm                    " 在处理未保存或只读文件的时候，弹出确认
set autoindent                 " 自动缩进
set cindent                    " 开启C语言语法自动缩进
set tabstop=4                  " Tab键的宽度
set softtabstop=4              " 统一缩进为4
set shiftwidth=4               " 设定 << 和 >> 命令移动时的宽度为 4
set noexpandtab                " 不要用空格代替制表符
set smarttab                   " 在行和段开始处使用制表符
set number                     " 显示行号
set cursorline                 " 突出显示当前行
set autochdir                  " 自动切换当前目录为当前文件所在的目录
set ruler                      " 打开状态栏标尺
set history=1000               " 历史记录数
set nobackup                   " 文件覆盖时不备份
set backupcopy=yes             " 设置备份时的行为为覆盖
filetype plugin indent on      " 开启插件
set showmatch                  " 插入括号时，短暂地跳转到匹配的对应括号
set matchtime=2                " 短暂跳转到匹配括号的时间
set hidden                     " 允许在有未保存的修改时切换缓冲区，此时的修改由 vim 负责保存
set guioptions-=T              " 隐藏工具栏
set guioptions-=m              " 隐藏菜单栏
set smartindent                " 开启新行时使用智能自动缩进
set noswapfile                 " 不使用swap文件
set ignorecase                 " 搜索忽略大小写
set hlsearch                   " 搜索时高亮显示被找到的文本
set incsearch                  " 输入搜索内容时就显示搜索结果
set gdefault                   " 行内替换
set enc=utf-8                  " 编码设置
set fencs=utf-8,ucs-bom,shift-jis,gb18030,gbk,gb2312,cp936
set langmenu=zh_CN.UTF-8       " 语言设置，中文UTF-8
set helplang=cn                " 执行help显示的语言
set guifont=Luxi/ Mono/ 9      " 设置字体，字体名称和字号"
```
​	对于更多的vim配置参数，请在vim手册上查看，用户自定义的配置推荐新建一个vim的配置文件`/home/$USER/.vim.config`，然后在`/etc/vim/vimrc`引入即可！
```
" 通过source加载引入自定义并可用的vim配置
if filereadable("/home/$USER/.vim.config")
  source /home/$USER/.vim.config
endif
```

**vim使工作模式**
​	在学习vim的用法之前，我们先了解vim的工作模式，vim编辑器有多种工作模式，比较常用的工作模式有四种：正常模式command mode）、编辑模式（insert mode）、命令行模式（command-line mode）、可视模式 (Visual model)。

正常模式：该模式为进入vim后的最初状态的模式，在该模式下，可以进行光标的移动，复制粘贴等。该模式下键盘上所有的健都为功能健，即该健已经不代表该键字面的意思，而是代表了某种功能。譬如`dd`键代表的就是删除当前一行的数据信息。

编辑模式：该模式为编辑文本正常的模式，就和平常的编辑器一样，在该模式下，vim在终端的左下角会出现一个Insert，然而`dd`键就代表输入`dd`两个字母。

命令行模式：该模式和一般模式看起来很像，也是输入命令，但是这种模式下的命令是在左下角可见的，按`Esc`然后按住`Shift`+`:`即可进入该模式，并且该模式下的命令需要按下`enter`回车才会执行。

可视模式：可视模式中的操作有点像拿鼠标进行操作，选择文本的时候有一种鼠标选择的即视感，在此模式下vim左下角会显示`--  可视  块 --`

这四种模式进入的操作如下表：

|  模式   | 操作                                       |
| :---: | ---------------------------------------- |
| 正常模式  | 按下`Esc`键即可进入正常模式                         |
| 编辑模式  | 在正常模式的前提下，按下`i 、I、a、A、o、O、r、R、s、S`其中一个按键即可进入编辑模式 |
| 命令行模式 | 按`Esc`然后按住`Shift`+`:`即可进入命令行模式           |
| 可视模式  | 按`Shift`+`v`/`V`即可进入可视模式                 |


**vim使用**
​	虽然说vim是一款跨平台、易扩展、效率高的编辑器，同时vim也是一款难上手的编辑器，毕竟vim的功能如此强大又那么简洁，如果你经常在Linux平台上开发与运维的，那么学习vim是非常有必要的。

​	对与vim的四种模式我们已经弄清楚了，键盘上的每一个键在不同的模式上都有不同的作用，那现在简单说明一些vim最常用的键以及它的作用，详细的说明可以在官方文档查阅，同时还可以在终端执行`vimtutor`即可看到vim的使用教程说明，但是对于初学者来说，更应该掌握如何去学习，大多数的命令都有help方法，vim也不例外，我们可以通过--help获取简要的使用信息。

```
➜  ~ vim -help
VIM - Vi IMproved 7.4 (2013 Aug 10, compiled Nov 24 2016 16:44:48)

用法: vim [参数] [文件 ..]       编辑指定的文件
  或: vim [参数] -               从标准输入(stdin)读取文本
  或: vim [参数] -t tag          编辑 tag 定义处的文件
  或: vim [参数] -q [errorfile]  编辑第一个出错处的文件

参数:
   --			在这以后只有文件名
   -v			Vi 模式 (同 "vi")
   -e			Ex 模式 (同 "ex")
   -E			Improved Ex mode
   -s			安静(批处理)模式 (只能与 "ex" 一起使用)
   -d			Diff 模式 (同 "vimdiff")
   -y			容易模式 (同 "evim"，无模式)
   -R			只读模式 (同 "view")
   -Z			限制模式 (同 "rvim")
# 输入比较多，此处省略... ...
```
```

```
下面就分几类来说明键的作用。

**光标移动**

|          键           | 说明        |
| :------------------: | :-------- |
|        j 或 ↓         | 下移一行      |
|        j 或 ↓         | 下移一行      |
|        k 或 ↑         | 上移一行      |
|      h 或 退格 或 ←      | 左移一个字符    |
|      l 或 空格 或 →      | 右移一个字符    |
|       0 或 Home       | 移动到本行的最前面 |
|       $ 或 End        | 移动到本行的最后面 |
|  Ctrl + b 或 Page Up  | 向上翻页      |
| Ctrl + f 或 Page Down | 向下翻页      |
|       Ctrl + u       | 向上翻半页     |
|       Ctrl + d       | 向下翻半页     |
|       Ctrl + e       | 向下滚动一行    |
|       Ctrl + y       | 向上滚动一行    |

查找替换*

|       键       | 说明              |
| :-----------: | :-------------- |
|     /word     | 向光标之后查找“word”   |
|     ?word     | 向光标之前查找“word”   |
|       n       | 重复前一个查找的动作      |
|       N       | 反向重复前一个查找的动作    |
|  :s/old/new   | 用new替换当前行第一个old |
| :s/old/new/g  | 用new替换当前行所有的old |
| :%s/old/new/g | 用new替换文件中所有的old |

删除(剪切)、复制、粘贴

|    键     | 说明               |
| :------: | :--------------- |
|    x     | 向后删除一个字符         |
|    X     | 向前删除一个字符         |
|  dd（yy）  | 删除（复制）光标所在的一整行   |
| d1G（y1G） | 删除（复制）光标以上所有行    |
|  dG（yG）  | 删除（复制）光标一下所有行    |
|  d0（y0）  | 删除（复制）光标到行首所有内容  |
|  d$（y$）  | 删除（复制）光标到行尾的所有内容 |
|    p     | 粘贴在光标所在行的下一行     |
|    P     | 粘贴在光标所在行的上一行     |
|    u     | 撤销命令             |

命令模式下的指令

|    键    | 说明                     |
| :-----: | :--------------------- |
|   :e    | 关闭当前文件并打开文件            |
|   :e+   | 关闭当前文件并打开文件，并从文件末尾开始编辑 |
|   :ls   | 列出所有打开的文件的列表           |
|   :n    | 切换到下一个被打开的文件           |
|   :N    | 切换到上一个被打开的文件           |
|   :w    | 保存，将文档保存在硬盘中           |
|   :q    | 退出vim                  |
|   :wq   | 保存并退出vim               |
|  :wq!   | 强制保存并退出vim             |
| :n1,n2w | 将n1行到n2行的内容另存为在        |
| :saveas | 文件另存为                  |



![vim键盘图](http://upload-images.jianshu.io/upload_images/1678789-6e57bdc74dce6fe6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**终端小技巧 **
- help

  ​	在Linux系统中，学会如何去掌握命令的用法以及作用是尤其重要的，大多数的Linux命令都会有一个--help参数，即一个命令的使用帮助手册。类似的还有man、info。

- Tab

  ​	Tab即自动补全，在Terminal敲命令的时候配合Tab键会使你的工作效率大大提高，比如，当你敲的命令太长或者你只记得命令的前面部分，只要按下Tab键，命令即可补全，还有一种情况，当你按下Tab键没有反应，那么就是说，你敲的命令与其他命令的前面部分相同，此时可以在此按下Tab键，即可显示所有与已敲部分命令开头的命令。
- history

  ​	在Linux终端执行命令的工作中，每次执行的命令都会被记录下来的，但是我们想想，既然已经记载我们所执行过的命令，我们再次执行相同的命令也就不需要再次输入重复的命令了，是的。我们可以配合  ↑、↓ 键使用，↑ 代表查找上一条命令，↓代表查找下一条命令，但是如果你想查看最近所执行的所有命令，可以使用`history`命令。

```
➜  ~ history
    1  ls
    2  vim --help
    3  chmod a+x email.sh 
    4  history 
```
- find
  ​	find顾名思义就是用于查找文件的，当我们想使用vim编辑一个配置文件，但是问题我们不知道此文件的路径位置，那我我们就可以使用find命令来查找该文件在哪。类似的命令还有which。

```
# 比如我们在当前的用户目录下查找一个文件名为email.sh的文件
➜  ~ find /home/$USER/ -name email.sh
/home/alic/email.sh
/home/alic/Alic/email.sh
```
- clear

  ​	在终端执行的命令，都会有大量的信息在屏幕显示出来，在很多时候都是好事，但是有时候执行完了命令后，看到大量的信息会觉得特别乱，此时我们可以使用clear清屏功能，在所在的终端输入clear或者`Ctrl+l`即可清空屏幕，还有类似的一个命令reset，与clear的区别就是，reset相当于从新开启了一个终端窗口。

___



**Emacs简介**

​	Emacs是一种功能超强的文本处理程序工具，与上述的vim类似，Emacs也是一款文本编辑器，在Windows、Mac、Linux等平台使用，以一款跨平台编辑器。在Unix/Linux界可以与vim媲美，对于哪一个比较好，人各有所好，都是Unix/Linux界的两大神奇。Emacs主要是基于C以及Emacs Lisp两种编程语言来写的。Emacs有一个很大的特点就是扩展性非常强，这种特性就是有Emacs Lisp语言来编写的。即使Emacs的功能在强大，它始终还是一个文本编辑器，与其它编程IDE还是有很大的不同的。由于Emacs的社区非常活跃，很多贡献与Emacs，因而Emacs还是可以工作很多事的，比如收发电子邮件、通过FTP/TRAMP编辑远程档案、通过Telnet登录主机、上新闻组、登陆IRC和朋友交流、查看日历、撰写文章大纲、对多种编程语言的编辑、调试程序、玩游戏、计算器、记日记、管理日程、个人信息管理、目录管理、文件比较、阅读info和man文档、浏览网站等诸的功能。
GUN Emacs编辑器有七大特性：
(1) 完全支持语法高亮
(2) 内置完整的使用教程
(3) 几乎支持所有的编码
(4) 高度可定制、扩展性高
(5) 超文本编辑器，可以作为调试接口等其它工具
(6) 用于下载和安装扩展的打包系统
(7) 窗口可分割，包括横、竖

![Emacs界面](http://upload-images.jianshu.io/upload_images/1678789-6bd843adb64cdff6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**Emacs安装**
​	在Emacs官网中，官方推荐我们使用包管理的方式进行安装，很多Linux的发行版中的包管理源已经存在了Emacs，除非我们希望使用最新版本的Emacs编辑器，可以在官方网站下载源码在本地变异安装。
基于Debian的Linux发行版的系统安装方式如下，比如Ubuntu系统：
```
$ apt-get update
$ apt-get install emacs
# 安装过程省略...
```

基于Red Hat的Linux发行版的系统安装方式如下，比如CentOS系统：
```
$ yum update
$ yum install emacs
# 安装过程省略...
```

基于pacman包管理器的Linux发行版安装方式如下，比如Arch Linux系统：
```
# 对于64位安装
pacman -S mingw-w64-x86_64-emacs
# 对于32位安装
pacman -S mingw-w64-i686-emacs
# 安装过程省略...
```

安装完毕之后我们可以在终端通过`emacs -version`命令来查看vim的安装版本。
```
➜  ~ emacs -version
GNU Emacs 24.3.1
Copyright (C) 2013 Free Software Foundation, Inc.
GNU Emacs comes with ABSOLUTELY NO WARRANTY.
You may redistribute copies of Emacs
under the terms of the GNU General Public License.
```

**emacs配置**
​	Emacs的配置与其它的配置文件很不同，与vim的配置类似，但是Emacs配置文件的语法格式更能体现出编程，并且Emacs的配置语法就是使用其Lisp的语法来约束规则的。也许你可能会奇怪，一般普通的程序软件配置，基本都是通过菜单栏打开一个settings选项来配置的，然后修改里面提供的选项，这是界面化的修改，偏向Windows系统的设置，然而在emacs里配置呢更能侧向Uinx/Linux系统。既然配置都是使用Lisp语法来配置的，那我们就接触一下简单的Lisp语法。
```
 ;; 这是行注释
 set 'a 5  ;;变量赋值
(add-to-list 'load-path (expand-file-name "~/.emacs.d")) ;; 如下引入一个文件目录
(require 'init-utils) ;; 加载init-utils自定义的函数和宏
```

​	Emacs的配置方法细分可以分类两种，与vim类似。
​	其一，单个文件作为Emacs 的配置文件，具体来说呢，就是将所有初始化函数都放在一个文件配置文件里即~/.emacs，虽然这样配置起来简单，但是你长时间使用Emacs编辑器的话，要配置的选项也随之越来越多的，这样一来，Emacs的配置文件就会越来越大，非常膨胀显得配置及其杂乱，这算是体现了面向过程的做法，这种配置的方法不推荐。
​	其二，一个文件夹作为Emacs的配置目录，详细来说呢，就是将配置的选项归纳分类在不同的文件里面，放在同一个目录下即~/.emacs.d，在该目录下的所有配置文件的扩展名都是  `.el`，这是Emacs编辑器加载配置文件的规则，Emacs在启动的时候会自动执行该目录下名为init.el的配置文件，但是init.el配置文件又依赖其他的文件，自然而然也就会加载其他的配置文件，这样就会显得更加清晰、简洁。这种做法更能体现出面向对象的特征，这是我推荐的配置方法。
​	这两种方法的配置参数加载是有顺序的，emacs启动时先加载~/.emacs配置文件，倘若~/.emacs配置文件不在的话，enacs会去~/.emacs.d目录下加载init.el配置文件。
为了方便展示，下面以一个文件的形式来说明emacs的基本常用配置项：
```
;; =================================
;; 外观配置项
;; =================================
;; 设置字体为Monospace  
(set-default-font "Monospace") 
;; 设置语法高亮显示的背景和前景，区域选择的背景和主题，二次选择的背景和选择 
(set-background-color "black")  
(set-foreground-color "white")
(set-face-foreground 'region "green")
(set-face-background 'region "blue")
(set-face-foreground 'secondary-selection "skyblue")
(set-face-background 'secondary-selection "darkblue")
;; 去掉侧边滚动条  
(set-scroll-bar-mode nil)  
;; 关闭开启画面  
(setq inhibit-startup-message t)  
;; 禁用工具栏  
(tool-bar-mode nil)  
;; 禁用菜单栏  
(menu-bar-mode nil)  
;; 移除警告
(mouse-wheel-mode t)  
;; 自动加载行号  
(global-linum-mode 'linum-mode)  
;; 高亮当前行  
(require 'hl-line-settings)  
;; 将光标由方块变成一个小长条  
(require 'bar-cursor)
;; 实现全屏效果，快捷键为f11
(global-set-key [f11] 'fullscreen) 
(defun fullscreen ()
(interactive)
(x-send-client-message
nil 0 nil "_NET_WM_STATE" 32
'(2 "_NET_WM_STATE_FULLSCREEN" 0))
)

;; =================================
;; 状态栏配置项
;; =================================
;;显示时间  
(display-time)  
;; 启用时间显示设置
(display-time-mode 1)
(setq display-time-24hr-format t)  ;; 使用24小时制  
(setq display-time-day-and-date t) ;; 显示包括日期和具体时间  
;;(setq display-time-use-mail-icon t) ;;时间栏旁边启用邮件设置  
;;(setq display-time-interval 10) ;;时间的变化频率
;;显示列号  
(setq column-number-mode t)  
;;显示标题栏 %f 缓冲区完整路径 %p 页面百分数 %l 行号  
(setq frame-title-format "%f")  

;; =================================
;; 编辑器配置项
;; =================================
;; 启用部分补全功能 
(partial-completion-mode 1)  
;; 在minibuffer里启用自动补全函数和变量 
(icomplete-mode 1)   
;; 不创建备份文件  
(setq make-backup-files nil)  
;; 不创建临时文件  
(setq-default make-backup-files nil)  
;; 渲染当前屏幕语法高亮，加快显示速度  
(setq lazy-lock-defer-on-scrolling t)  
;; 将错误信息显示在回显区  
(condition-case err  
    (progn  
      (require 'xxx))  
  (error  
   (message "Can't load xxx-mode %s" (cdr err))))  
;; 使用X剪贴板  
(setq x-select-enable-clipboard t)  
;;设定剪贴板的内容格式
(set-clipboard-coding-system 'ctext)  
;; 设置Tab宽度为4  
(setq default-tab-width 4)   
;; 设置缩进  
(setq indent-tabs-mode t)  
(setq c-indent-level 4)  
(setq c-continued-statement-offset 4)  
(setq c-brace-offset -4)  
(setq c-argdecl-indent 4)  
(setq c-label-offset -4)  
(setq c-basic-offset 4)  
;; 设定行距  
(setq default-line-spaceing 4)  
;; 设定页宽  
(setq default-fill-column 60)  
;; 缺省模式 text-mode  
(setq default-major-mode text-mode)  
;; 设置删除记录最大值
(setq kill-ring-max 200)  
;; 以空行结束  
(setq require-final-newline t)  
;; 鼠标指针避光标  
(set mouse-avoidance-mode animate)  
;; 粘贴于光标处,而不是鼠标指针处  
(setq mouse-yank-at-point t) 
```
以上并非Emacs编辑器的完整所有的配置项，更多的配置项请查看官方网站或emacs插件的使用说明。

**Emacs使用 **
​	emacs与vim类似，只要多操作使用就自然会记住每个键或键键之间的作用，以下是emacs常用的操作快捷键。在Emacs编辑器中，一些默认快捷键`C-`代表按住`Ctrl键`，`M-`代表按住`Alt键`。对于简单的入门，熟悉使用如下的键键配合使用即可，但是想一时掌握Emacs的话，这是不可能的，需要长期的使用才会掌握Emacs的熟练使用。

光标移动

|           键           | 说明                             |
| :-------------------: | :----------------------------- |
|       C-n/p/b/f       | 到 下一行 / 上一行 / 前一字符 / 后一字符      |
|         M-b/f         | 到 前 / 后一单词                     |
|         C-a/e         | 到 行首 / 末                       |
|         M-a/e         | 到 句首 / 末                       |
|        C-v/M-v        | 下 / 上翻屏                        |
|          C-l          | 循环 将当前光标行显示在窗口下、中、上位置          |
|          C-u          | 重复执行之后的 xx 命令 num 次，不输入num默认为4 |
| C-u C-v        向下滚动4行 |                                |

编辑
|     键      | 说明         |
| :--------: | :--------- |
|  Back/C-d  | 删除 前 / 后字符 |
| M-Back/M-d | 移除 前 / 后单词 |
|  C-k/M-k   | 移除到 行末、句末  |
|   C-x u    | 撤销/重做      |
|    C-w     | 剪切         |
|    C-y     | 粘贴         |
|    C-c     | 复制         |

查找
|  键   | 说明          |
| :--: | :---------- |
| C-s  | 查找下一个       |
| C-r  | 查找上一下       |
| C-g  | 一次返回，二次结束查找 |

标记

|    键    | 说明            |
| :-----: | :------------ |
|   C-@   | 设置标记          |
| C-x C-x | 在当前光标位置和标记处跳转 |
|   M-@   | 设置单词标记        |
|   M-h   | 标记段           |
| C-x C-p | 标记页           |
|  C-M-@  | 标记表达式         |
|  C-M-h  | 标记函数          |
|  C-x h  | 标记整个缓冲区       |



查询替换
|  键   | 说明              |
| :--: | :-------------- |
| M-%  | 交互替换            |
| SPC  | 替换当前项并跳转到下一项    |
|  ,   | 替换当前项不跳转        |
| DEL  | 不替换当前项并跳转到下一项   |
|  !   | 替换全部剩下的查询结果     |
|  ^   | 回到上一个匹配结果       |
| RET  | 退出查询替换          |
| C-r  | 进入递归编辑(C-M-c退出) |



文件操作
|    键    | 说明                            |
| :-----: | :---------------------------- |
| C-x C-c | 退出                            |
| C-x C-f | 打开文件                          |
| C-x C-s | 保存文件                          |
| C-x C-v | 在当前缓冲区重新打开一个文件                |
|  C-x 1  | 最大化当前缓冲，关闭其它                  |
| C-x 2/3 | 垂直、水平创建新缓冲区                   |
|  C-x o  | 切换到其它缓冲区                      |
| C-x C-b | 列出缓冲区                         |
|  C-x b  | 提示输入缓冲区名称，切换当前窗口的缓冲区          |
|  C-x k  | 删除当前缓冲区                       |
|   C-j   | 换行回车，有些模式下比较直接回车好用，有增加缩进之类的功能 |

#### sed

**sed简介 **
​	sed(Stream EDitor)是一个通用的编辑器，它是Uinx/Linux内核bash内置的一个指令。目前由自由软件基金组织开发和维护并且随着GNU/Linux进行分发，通常它也称作 GNU SED。
Linux系统中虽然可以使用强大的编辑器如 vim 和 emacs 来编辑处理文本文件。但有往往有些时候，我们需要有一种命令自动的处理文本文件内容，并非使用这些强大的交互式文本编辑器，这时，sed命令式文本处理工具就排上用场了，它们能够轻松的实现自动格式化、插入、修改或者删除等操作处理文本信息。

**sed工作原理**
​	sex一次只能处理一行内容，处理时，把当前处理的行存储在临时缓冲区中，称为“模式空间”（pattern space），接着用sed命令处理缓冲区中的内容，处理完成后，把缓冲区的内容送往屏幕。接着处理下一行，这样不断重复，直到文件末尾。文件内容并没有 改变，sed只是对缓冲区中原始文件的副本进行编辑，并不是编辑原始的文件。除非你使用重定向存储输出或者使用使用sed编辑命令中的w选项。
Sed主要用来自动编辑一个或多个文件；简化对文件的反复操作。

![sed工作原理](http://upload-images.jianshu.io/upload_images/1678789-3ff3dde1e8094178.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**sed使用**
选项与参数说明：

|  参数  | 说明                     |
| :--: | :--------------------- |
|  -n  | 使用安静(silent)模式         |
|  -e  | 直接在命令列模式上进行 sed 的动作编辑  |
|  -f  | 直接将 sed 的动作写在一个文件内     |
|  -r  | sed 的动作支持的是延伸型正规表示法的语法 |
|  -i  | 直接修改读取的文件内容，而不是输出到终端   |

动作说明：

|  参数  | 说明                                    |
| :--: | :------------------------------------ |
|  a   | 新增， a 的后面可以接字串，而这些字串会在新的一行出现(当前行的下一行) |
|  c   | 取代， c 的后面可以接字串，这些字串可以取代 n1,n2 之间的行    |
|  d   | 删除， d 后面通常不接任何参数                      |
|  i   | 插入， i 的后面可以接字串，而这些字串会在新的一行出现(当前行的上一行) |
|  p   | 打印，通常 p 会与参数 sed -n 一起使用              |
|  s   | 替换，通常与 s 配合，即搭配正规表示法                  |

​	对于Linux运维者来说，学会命令行工具处理事务比可视化交互式处理事务尤其重要，一旦掌握了sed等命令的语法，你就可以轻松、智能地处理复杂的问题，并且是全自动处理，甚至有时只是几行代码就可以解决的，这样处理就可以大量减少处理事务的时间，提高处理效率！下面就实践一些在Linux运维中经常使用sed相关的命令:

### 示例
先建立一个sedfile文件，里面的内容如下：
```
我是老一
你是老二
他是老三
四哥，老大呢
```
- 在指定行添加行，比如在第三行添加一行数据`three`

```
# 在第三行前添加，即第三行
➜  ~ sed '3i three' sedfile 
我是老一
你是老二
three
他是老三
四哥，老大呢

# 在第二行后添加，即第三行
➜  ~ sed '2a three' sedfile
我是老一
你是老二
three
他是老三
四哥，老大呢
```

- 删除指定行内容，可以是一个范围的多行，从1开始数起，$表示最后一行

```
#删除第一行，没有-e参数也是可以的
➜  ~ sed -e '1d' sedfile
你是老二
他是老三
四哥，老大呢

#删除第二到第三行
➜  ~ sed -e '2,3d' sedfile
我是老一
四哥，老大呢

# 删除第二到最后一行
➜  ~ sed -e '2,$d' sedfile
我是老一
```

- 以行为单位替换内容

```
# 替换第一行的内容
➜  ~ sed '1c 老大不见了' sedfile
老大不见了
你是老二
他是老三
四哥，老大呢

#替换第一第二行的内容
➜  ~ sed '1,2c 老大老二都不见了' sedfile
老大老二都不见了
他是老三
四哥，老大呢
```

- 以行为单位的显示

```
# 显示第二到第三行内容
➜  ~ sed -n '2,3p' sedfile 
你是老二
他是老三

# 搜索有是的行的所有行内容
➜  ~ sed -n '/是/p' sedfile
我是老一
你是老二
他是老三
```

- 搜索并删除行内容

```
# 搜索`四哥`，并将其删除
➜  ~ sed '/四哥/d' sedfile
我是老一
你是老二
他是老三

```

- 搜索并替换内容

```
# 将`四哥`换成`alic``
➜  ~ sed 's/四哥/alic/g' sedfile
我是老一
你是老二
他是老三
alic，老大呢

```

- 多点处理，即灵活多步处理

```
# 删除第二行，并将 `四哥`换成`alic`
➜  ~ sed -e  '2d' -e 's/四哥/alic/g' sedfile 
我是老一
他是老三
alic，老大呢
```

- 对文件修改后进行保存，只要加上-i参数即可，这是危险的最法，可以不使用-i而将sed执行的结果重定向也可以，这样的做法还可以保留原文件

```
# 将`四哥`换成`alic`后并保存
➜  ~ sed -i 's/四哥/alic/g' sedfile         
# 查看上一次执行成功后，sedfile文件的内容
➜  ~ cat sedfile 
我是老一
你是老二
他是老三
alic，老大呢
```

























___