---
layout: reference
---

## 基本命令

| 动作         | 文件  | 目录          |
|-------------|-------|--------------|
| 查看信息     | ls    | ls           |
| 查看内容     | cat   | ls           |
| 导航        |       | cd           |
| 移动        | mv    | mv           |
| 拷贝        | cp    | cp -r        |
| 创建        | touch | mkdir        |
| 删除        | rm    | rmdir, rm -r |

## 文件系统结构

以下是标准Linux文件系统的概述。
确切的层次结构取决于平台。您的文件/目录结构可能略有不同：

![Linux文件系统层次结构](fig/standard-filesystem-hierarchy.svg)

## 词汇表

{:auto_ids}
绝对路径absolute path
:   一个 [path](#path)，它引用文件系统中的特定位置。
    绝对路径通常是相对于文件系统的
    [根目录](#root-directory),
    并以“/”（在Linux上）或“\\”（在 Microsoft Windows 上）开头。
    另请参阅：[相对路径](#relative-path)。

argument
:   函数或程序运行时赋予它的值。

命令shell
:   查看[shell](#shell)

命令行接口command line interface
:   基于键入命令的用户界面，
    通常在 [REPL](#read-evaluate-print-loop)。
    另请参阅：[图形用户界面](#graphical-user-interface)。

注释comment
:   旨在帮助人类读者理解正在发生的事情的程序中的注释，
    但被计算机忽略。
    Python、R 和 Unix shell 中的注释以 `#` 字符开头
    并跑到行尾；
    SQL 中的注释以 `--` 开头，
    其他语言有其他约定。


当前工作目录current working directory
:   [相对路径](#relative-path) 计算的目录；
    等效地，
    仅搜索按名称引用的文件的位置。
    每个 [process](#process) 都有一个当前工作目录。
    当前工作目录通常使用简写符号 `.` 来表示。
    （发音为“点”）。

文件系统file system
:   一组文件、目录和 I/O 设备（例如键盘和屏幕）。
    一个文件系统可能分布在许多物理设备上，
    或者许多文件系统可以存储在单个物理设备上；
    [操作系统](#operating-system) 管理访问。

文件后缀file extension
:   文件名中最后一个“.”之后的部分。
    按照惯例，这标识了文件的类型：
    `.txt` 表示“文本文件”，`.png` 表示“便携式网络图形文件”，
    等等。 大多数操作系统不强制执行这些约定：
    将 MP3 声音文件命名为“homepage.html”是完全可能的（但令人困惑！）。
    由于许多应用程序使用文件扩展名来识别
    [MIME 类型](#mime-type) 文件，
    错误命名的文件可能会导致这些应用程序失败。

过滤filter
:   转换数据流的程序。
    许多Linux命令行工具被编写为过滤器：
    他们从 [标准输入](#standard-input) 读取数据，
    处理它，并将结果写入[标准输出](#standard-output)。

for loop循环
:   对某种集合、列表或范围中的每个值执行一次的循环。
    另请参阅：[while 循环](#while-loop)。

图形用户接口graphical user interface
:   基于从图形显示中选择项目和操作的用户界面，
    通常使用鼠标控制。
    另请参阅：[命令行界面](#command-line-interface)。

家目录home directory
:   与计算机系统上的帐户关联的默认目录。
    按照惯例，用户的所有文件都存储在她的主目录中或之下。

循环loop
:   一组要执行多次的指令。
    由一个 [循环体](#loop-body) 和（通常）一个
    退出循环的条件。 另见 [for 循环](#for-loop) 和 [while 循环](#while-loop)。

循环体loop body
:   在 [for 循环](#for-loop) 内重复的一组语句或命令
    或 [while 循环](#while-loop)。

MIME type类型
:   MIME（多用途 Internet 邮件扩展）类型描述用于交换的不同文件类型
    例如，图像、音频和文档。

操作系统operating system
:   管理用户、硬件和软件 [进程](#process) 之间交互的软件。
    常见的例子是 Linux、macOS 和 Windows。

选项option
:   一种为命令行程序指定参数或设置的方法。
    按照惯例，Unix 应用程序使用破折号后跟一个字母，
    例如 `-v`，或两个破折号后跟一个单词，例如 `--verbose`，
    而 DOS 应用程序使用斜杠，例如 `/V`。
    根据应用程序，一个选项后面可能跟一个参数，
    如`-o /tmp/output.txt`。

参数parameter
:   在函数声明中命名的变量，用于保存传递给调用的值。
    该术语经常与 [argument](#argument) 互换使用（且不一致）。

父目录parent directory
:   “包含”有问题的目录。
    除了[根目录](#root-directory)，文件系统中的每个目录都有一个父目录。
    目录的父级通常使用简写符号“..”来表示
    （发音为“点点”）。

路径path
:   指定文件或目录在一个文件或目录中的位置的描述
    [文件系统](#file-system)。
    另请参阅：[绝对路径](#absolute-path)、[相对路径](#relative-path)。

管道pipe
:   从一个程序的输出到另一个程序的输入的连接。
    当两个或多个程序以这种方式连接时，它们被称为“管道”。

进程process
:   程序的运行实例，包含代码、变量值、
    打开文件和网络连接等等。
    进程是[操作系统](#operating-system) 管理的“参与者”；
    它通常一次运行每个进程几毫秒
    给人一种他们同时执行的印象。

提示prompt
:   一个或多个字符由 [REPL](#read-evaluate-print-loop) 显示以表明
    它正在等待下一个命令。

引号quoting
:   （在shell中）：
     使用各种引号来防止shell解释特殊
     人物。
     例如，要将字符串 `*.txt` 传递给程序，
     通常需要写成`'*.txt'`（带单引号）
     这样 shell 就不会尝试扩展 `*` 通配符。

读-验证-打印循环read-evaluate-print loop
:   (REPL)：从用户那里读取命令的[命令行接口](#命-令-行-接-口)，
    执行它，打印结果，然后等待另一个命令。

重定向redirect
:   要将命令的输出发送到文件而不是屏幕或其他命令，
    或等效地从文件中读取命令的输入。

正则表达式regular expression
:   指定一组字符串的模式。
    RE最常用于查找字符串中的字符序列。

相对路径relative path
:   [path](#path) 指定文件或目录的位置
    关于[当前工作目录]（#current-working-directory）。
    任何不以分隔符（“/”或“\\”）开头的路径都是相对路径。
    另请参阅：[绝对路径](#absolute-path)。

根目录root directory
:   [文件系统](#file-system) 中最顶层的目录。
    它的名称在 Unix（包括 Linux 和 macOS）上是“/”，在 Microsoft Windows 上是“\\”。

shell
:   [命令行界面](#cli)，例如 Bash（Bourne-Again Shell）
    或 Microsoft Windows DOS shell
    允许用户与[操作系统]（#operating-system）交互。

shell script
:   存储在文件中以供重复使用的一组 [shell](#shell) 命令。
    shell脚本是由shell执行的程序；
    出于历史原因，使用“脚本”这个名称。

标准输入standard input
:   进程的默认输入流。
    在交互式命令行应用程序中，
    它通常连接到键盘；
    在 [管道](#pipe) 中，
    它从前面进程的 [标准输出](#standard-output) 接收数据。

标准输出standard output
:   进程的默认输出流。
    在交互式命令行应用程序中，
    发送到标准输出的数据显示在屏幕上；
    在 [管道](#pipe) 中，
    它被传递到下一个进程的 [标准输入](#standard-input)。

子目录sub-directory
:   包含在另一个目录中的目录。

tab completion补全
:   许多交互式系统提供的一项功能，其中
    按Tab键会触发当前单词或命令的自动完成。

变量variable
:   程序中与一个值或一组值相关联的名称。

while loop循环
:   只要某些条件为真，循环就会一直执行。
    另请参阅：[for 循环](#for-loop)。

通配符wildcard
:   用于模式匹配的字符。
    在Linux shell中，
    通配符 `*` 匹配零个或多个字符，
    这样 `*.txt` 匹配所有名称以 `.txt` 结尾的文件。

## 外部参考

### 打开一个终端
* [如何在 Mac 上使用终端](http://www.macworld.co.uk/feature/mac-software/how-use-terminal-on-mac-3608274/)
* [Git for Windows](https://git-for-windows.github.io/)
* [如何在 Windows 10 上安装 Bash shell 命令行工具](https://www.windowscentral.com/how-install-bash-shell-command-line-windows-10)
* [在 Windows 10 上安装和使用 Linux Bash Shell](https://www.howtogeek.com/249966/how-to-install-and-use-the-linux-bash-shell-on-windows-10/ )
* [使用 Windows 10 Bash Shell](https://www.howtogeek.com/265900/everything-you-can-do-with-windows-10s-new-bash-shell/)
* [使用 UNIX/Linux 模拟器 (Cygwin) 或 Secure Shell (SSH) 客户端 (Putty)](http://faculty.smu.edu/reynolds/unixtut/windows.html)

### 用户手册
* [GNU 手册](http://www.gnu.org/manual/manual.html)
* [核心 GNU 实用程序](http://www.gnu.org/software/coreutils/manual/coreutils.html)

### 各种文章资源
* [北太平洋环流](http://en.wikipedia.org/wiki/North_Pacific_Gyre)
* [大太平洋垃圾补丁](http://en.wikipedia.org/wiki/Great_Pacific_Garbage_Patch)
* [杰夫·罗森伯格的“确保数字信息的寿命”](http://www.clir.org/pubs/archives/ensuring.pdf)
* [计算机错误Haiku](http://wiki.c2.com/?ComputerErrorHaiku)
* [如何很好地命名文件，作者 Jenny Bryan](https://speakerdeck.com/jennybc/how-to-name-files)
