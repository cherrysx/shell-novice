---
layout: page
title: Linux Shell环境设置
root: .
---

## 下载文件
您需要下载一些文件来学习本课程。

1. 关注“源码工程师”公众号，回复“hpc1”，下载shell-lesson-data.zip并将文件移动到您的桌面。
2. 解压/解压文件。
   **如果您在此步骤中需要帮助，请告知**。
   您应该在桌面上创建一个名为**`shell-lesson-data`**的新文件夹。

## 安装软件
如果您尚未安装shell软件，则需要关注“源码工程师”公众号，回复“hpc2”，下载安装shell软件

## 打开一个新的shell
安装软件后：
1. 打开一个终端
   如果您不确定如何在操作系统上打开终端，请参阅以下说明。
2. 在终端输入 `cd` 然后按 <kbd>Return</kbd> 键。
   此步骤将确保您从主文件夹开始作为工作目录。

在本课程中，您将了解如何访问此文件夹中的数据文件。

> ## 在哪里输入命令：如何打开一个新的shell
>
> Shell是一个程序，它使我们能够向计算机发送命令并接收输出。
> 它也称为终端或命令行。
>
> 某些计算机包含默认的Linux Shell程序。
> 下面的步骤描述了一些识别和打开的方法
> 如果您已经安装了一个Linux Shell程序。
> 还有用于识别和下载Linux Shell程序的选项，
> Linux模拟器，或访问服务器上的Linux Shell的程序。
>
> 如果以下选项均不能解决您的情况，
> 尝试在线搜索：Linux shell [您的计算机型号] [您的操作系统]。
{: .callout}

{::options parse_block_html="true" /}
<div>
<ul class="nav nav-tabs nav-justified" role="tablist">
<li role="presentation" class="active"><a data-os="windows" href="#windows" aria-controls="Windows"
role="tab" data-toggle="tab">Windows</a></li>
<li role="presentation"><a data-os="macos" href="#macos" aria-controls="macOS" role="tab"
data-toggle="tab">macOS</a></li>
<li role="presentation"><a data-os="linux" href="#linux" aria-controls="Linux" role="tab"
data-toggle="tab">Linux</a></li>
</ul>

<div class="tab-content">
<article role="tabpanel" class="tab-pane active" id="windows">
具有Windows操作系统的计算机不会自动具有Linux Shell程序。
在本课中，我们鼓励您使用Git for Windows中包含的模拟器，您应该已经下载。
它使您可以访问Bash shell命令和Git。

安装后，您可以通过从Windows启动运行程序Git Bash打开终端菜单。

**对于高级用户:**

作为Windows版Git的替代方案，您可能希望 [安装Windows Linux子系统][wsl]
它可以访问Windows 10中的 Bash shell 命令行工具。

请注意，适用于Linux的Windows子系统 (WSL) 中的命令可能略有不同
来自课程中展示或研讨会中展示的内容。
</article>

<article role="tabpanel" class="tab-pane" id="macos">
对于运行 macOS Mojave 或更早版本的 Mac 计算机，默认的 Unix Shell 是 Bash。
对于运行 macOS Catalina 或更高版本的 Mac 计算机，默认的 Unix Shell 是 Zsh。
您的默认shell可通过 Utilities 文件夹中的终端程序获得。

要打开终端，请尝试以下一项或两项操作：
* 在 Finder 中，选择前往菜单，然后选择实用程序。
  在 Utilities 文件夹中找到终端并打开它。
* 使用 Mac 的“Spotlight”计算机搜索功能。
  搜索：`终端` 并按 <kbd>Return</kbd>。

要检查您的机器是否设置为使用 Bash 以外的其他工具，
在终端窗口中输入“echo $SHELL”。

如果你的机器被设置为使用 Bash 以外的东西，
您可以通过打开终端并输入“bash”来运行它。

[如何在 Mac 上使用终端][mac-terminal]
</article>

<article role="tabpanel" class="tab-pane" id="linux">
Linux 操作系统的默认Shell通常是 Bash。
在大多数版本的Linux上，可以通过运行
[Gnome Terminal][gnome-terminal]或[KDE Konsole][kde-konsole] 或 [xterm][xterm]，
可以通过应用程序菜单或搜索栏找到。
如果你的机器被设置为使用Bash以外的东西，
您可以通过打开终端并输入“bash”来运行它。
</article>
</div>
</div>

[wsl]: https://docs.microsoft.com/en-us/windows/wsl/install-win10
[mac-terminal]: http://www.macworld.co.uk/feature/mac-software/how-use-terminal-on-mac-3608274/
[gnome-terminal]: https://help.gnome.org/users/gnome-terminal/stable/
[kde-konsole]: https://konsole.kde.org/
[xterm]: https://en.wikipedia.org/wiki/Xterm
[install_shell]: https://carpentries.github.io/workshop-template/#shell
