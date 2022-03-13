---
title: "导航文件和目录"
teaching: 30
exercises: 10
questions:
- "如何在我的电脑上四处查看文件和目录？"
- "如何查看我拥有的文件和目录？"
- "如何指定计算机上文件或目录的位置？"
objectives:
- "解释文件和目录的异同。"
- "将绝对路径转换为相对路径，反之亦然。"
- "创建和访问特定文件和目录的绝对路径和相对路径。"
- "使用选项和参数来更改shell命令的行为。"
- "演示tab制表符补全的使用并解释其优点。"
keypoints:
- "文件系统负责管理磁盘上的信息。"
- "信息存储在文件中，这些文件存储在目录（文件夹）中。"
- "目录还可以存储其他目录，然后形成目录树。"
- "`cd [path]` 改变当前工作目录。"
- "`ls [path]` 打印特定文件或目录的列表；`ls` 单独列出当前工作目录。"
- "`pwd` 打印用户的当前工作目录。"
- "`/` 本身就是整个文件系统的根目录。"
- "大多数命令采用以 `-` 开头的参数。"
- "相对路径指定从当前位置开始的位置。"
- "绝对路径指定从文件系统的根开始的位置。"
- "路径中的目录名称在Linux上用 `/` 分隔，但在Windows上用 `\\` 分隔。"
- "`..` 表示“当前目录之上的目录（父目录）”； `.` 单独表示“当前目录”。"
---

操作系统中负责管理文件和目录的部分
被称为**文件系统**。
它将我们的数据组织成文件，
保存信息，
和目录（也称为“文件夹”），
保存文件或其他目录。

有几个命令经常用于创建、检查、重命名和删除文件和目录。
要开始探索它们，我们将转到打开的 shell 窗口。

首先，让我们通过运行名为 `pwd` 的命令找出我们在哪里
（代表“打印工作目录”）。 目录就像 *places* — 在任何时候
当我们使用 shell 时，我们正好在一个地方叫
我们的**当前工作目录**。 命令主要读取和写入文件中的
当前工作目录，即“这里”，所以在运行之前知道你在哪里
命令很重要。 `pwd` 显示你在哪里：

~~~
$ pwd
~~~
{: .language-bash}

~~~
/Users/nelle
~~~
{: .output}

这里，
计算机的响应是`/Users/nelle`，
这是 Nelle 的**主目录**：

> ## 主目录变体
>
> 主目录路径在不同的操作系统上看起来会有所不同。
> 在 Linux 上，它可能看起来像 `/home/nelle`，
> 在 Windows 上，它将类似于 `C:\Documents and Settings\nelle` 或
> `C:\Users\nelle`。
>（请注意，不同版本的 Windows 看起来可能略有不同。）
> 在以后的示例中，我们使用 Mac 输出作为默认输出 - Linux 和 Windows
> 输出可能略有不同，但应大致相似。
>
> 我们还将假设您的 `pwd` 命令返回用户的主目录。
> 如果 `pwd` 返回不同的内容，您可能需要使用 `cd` 导航到那里
> 或本课中的某些命令将无法正常工作。
> 详情请参阅[探索其他目录](#exploring-other-directories)
> 在 `cd` 命令上。
{: .callout}

要了解什么是“主目录”，
让我们看一下整个文件系统是如何组织的。为了
为了这个例子，我们将
说明我们科学家 Nelle 计算机上的文件系统。在这之后
插图，您将学习命令来探索自己的文件系统，
它将以类似的方式构造，但不完全相同。

在 Nelle 的计算机上，文件系统如下所示：

![文件系统由包含子目录的根目录组成
标题为 bin、data、users 和 tmp](../fig/filesystem.svg)

顶部是**根目录**
拥有其他一切。
我们单独使用斜线字符`/`来引用它；
该字符是 `/Users/nelle` 中的前导斜杠。

该目录内还有其他几个目录：
`bin`（存储一些内置程序的地方），
`data`（用于杂项数据文件），
`Users`（用户的个人目录所在的位置），
`tmp`（用于不需要长期存储的临时文件），
等等。

我们知道我们当前的工作目录 `/Users/nelle` 存储在 `/Users` 中
因为 `/Users` 是其名称的第一部分。
相似地，
我们知道`/Users`存储在根目录`/`中
因为它的名字以`/`开头。

> ## 斜线
>
> 请注意，`/` 字符有两种含义。
> 当它出现在文件或目录名的前面时，
> 它指的是根目录。 当它出现在*内部*路径中时，
> 它只是一个分隔符。
{: .callout}

在“/用户”下，
我们为每个在 Nelle 机器上拥有帐户的用户找到一个目录，
她的同事 *imhotep* 和 *larry*。

![和其他目录一样，主目录是下面的子目录
“/Users”，例如“/Users/imhotep”、“/Users/larry”或
“/Users/nelle”](../fig/home-directories.svg)

用户 *imhotep* 的文件存储在 `/Users/imhotep` 中，
用户 *larry* 在 `/Users/larry` 中，
而 Nelle 在 `/Users/nelle` 中。 因为 Nelle 是我们的用户
这里是示例，因此我们将 `/Users/nelle` 作为我们的主目录。
通常，当您打开一个新的命令提示符时，您将在
你的主目录开始。

现在让我们学习让我们看到我们的内容的命令
自己的文件系统。 我们可以通过运行 `ls` 查看主目录中的内容：

~~~
$ ls
~~~
{: .language-bash}

~~~
Applications Documents    Library      Music        Public
Desktop      Downloads    Movies       Pictures
~~~
{: .output}

（同样，根据您的操作，您的结果可能会略有不同
系统以及您如何自定义文件系统。）

`ls` 打印当前目录中的文件和目录的名称。
我们可以通过使用 `-F` **选项**使其输出更易于理解
它告诉`ls`对输出进行分类
通过在文件和目录名称中添加标记来指示它们是什么：
- 结尾的 `/` 表示这是一个目录
- `@` 表示链接
- `*` 表示可执行文件

根据您的 shell 的默认设置，
shell 也可能使用颜色来指示每个条目是文件还是
目录。

~~~
$ ls -F
~~~
{: .language-bash}

~~~
Applications/ Documents/    Library/      Music/        Public/
Desktop/      Downloads/    Movies/       Pictures/
~~~
{: .output}

这里，
我们可以看到我们的主目录只包含**子目录**。
我们输出中没有分类符号的任何名称
是普通的旧**文件**。

> ## 清除终端
>
> 如果您的屏幕太杂乱，您可以使用
> `clear` 命令。 您仍然可以使用 <kbd>↑</kbd> 访问以前的命令
> 和 <kbd>↓</kbd> 逐行移动，或在终端中滚动。
{: .callout}

### 帮助

`ls` 还有很多其他的**选项**。 有两种常见的方法来找出如何
使用命令及其接受的选项 ---
**根据您的环境，您可能会发现只有以下一种方式有效：**

1. 我们可以将 `--help` 选项传递给命令（在 macOS 上不可用），例如：
    ~~~
    $ ls --help
    ~~~
    {: .language-bash}

2. 我们可以使用 `man` 阅读它的手册（在 Git Bash 中不可用），例如：
    ~~~
    $ man ls
    ~~~
    {: .language-bash}

接下来我们将描述这两种方式。

#### `--help` 选项

人们编写的大多数 bash 命令和程序
从 bash 中运行，支持显示更多信息的 `--help` 选项
有关如何使用命令或程序的信息。

~~~
$ ls --help
~~~
{: .language-bash}

~~~
用法：ls [选项]... [文件]...
列出有关文件的信息（默认为当前目录）。
如果既没有指定 -cftuvSUX 也没有指定 --sort，则按字母顺序对条目进行排序。

多头期权的强制性参数对于空头期权也是强制性的。
  -a, --all 不要忽略以 . 开头的条目。
  -A, --almost-all 不列出隐含的。和 ..
      --author with -l，打印每个文件的作者
  -b, --escape 打印非图形字符的 C 样式转义
      --block-size=SIZE 在打印前按 SIZE 缩放尺寸；例如。，
                               '--block-size=M' 以单位打印尺寸
                               1,048,576 字节；请参阅下面的 SIZE 格式
  -B, --ignore-backups 不列出以 ~ 结尾的隐含条目
  -c 和 -lt：排序并显示 ctime（最后的时间
                               文件状态信息的修改）；
                               with -l: 显示 ctime 并按名称排序；
                               否则：按 ctime 排序，最新的在前
  -C 按列列出条目
      --color[=WHEN] 为输出着色； WHEN 可以是“总是”（默认
                               如果省略）、“自动”或“从不”；更多信息如下
  -d, --directory 列出目录本身，而不是它们的内容
  -D, --dired 生成为 Emacs 的 dired 模式设计的输出
  -f 不排序，启用 -aU，禁用 -ls --color
  -F, --classify 将指示符（*/=>@| 之一）附加到条目
...        ...        ...
~~~
{: .output}

> ## 不支持的命令行选项
> 如果您尝试使用不支持的选项，`ls` 和其他命令
> 通常会打印类似于以下内容的错误消息：
>
> ~~~
> $ ls -j
> ~~~
> {: .language-bash}
>
> ~~~
> ls: invalid option -- 'j'
> Try 'ls --help' for more information.
> ~~~
> {: .error}
{: .callout}

#### `man` 命令

了解 `ls` 的另一种方法是：
~~~
$ man ls
~~~
{: .language-bash}

此命令会将您的终端变成带有描述的页面
`ls` 命令及其选项。

要浏览“手册”页面，
您可以使用 <kbd>↑</kbd> 和 <kbd>↓</kbd> 来逐行移动，
或尝试使用 <kbd>B</kbd> 和 <kbd>Spacebar</kbd> 向上和向下跳过整页。
要在“手册”页中搜索字符或单词，
使用 <kbd>/</kbd> 后跟要搜索的字符或单词。
有时搜索会导致多次点击。
如果是这样，您可以使用 <kbd>N</kbd> 在点击之间移动（用于向前移动）和
<kbd>Shift</kbd>+<kbd>N</kbd>（用于向后移动）。

要**退出** `man` 页面，请按 <kbd>Q</kbd>。

> ## 网络上的手册页
>
> 当然，还有第三种获取命令帮助的方法：
> 通过您的网络浏览器搜索互联网。
> 使用 Internet 搜索时，在搜索中包括短语 `linux man page`
> 查询将有助于查找相关结果。
>
> GNU 提供了
> [手册](http://www.gnu.org/manual/manual.html) 包括
> [核心 GNU 实用程序](http://www.gnu.org/software/coreutils/manual/coreutils.html),
> 其中涵盖了本课中介绍的许多命令。
{: .callout}

> ## 探索更多 `ls` 标志
>
> 您也可以同时使用两个选项。 ls命令在使用时有什么作用
> 使用 `-l` 选项？ 如果你同时使用 `-l` 和 `-h` 选项呢？
>
> 它的一些输出是关于我们在本课中未涉及的属性（例如
> 作为文件权限和所有权），但尽管如此，其余的是有用的。
>
> > ## 解决方案
> > `-l` 选项使 `ls` 使用 **l**long 列表格式，不仅显示
> > 文件/目录名称以及附加信息，例如文件大小
> > 以及最后一次修改的时间。 如果同时使用 `-h` 选项和 `-l` 选项，
> > 这使得文件大小“**h**uman readable”，即显示类似 `5.3K` 的内容
> > 而不是“5369”。
> {: .solution}
{: .challenge}

> ## 按时间倒序排列
>
> 默认情况下，`ls` 按字母顺序列出目录的内容
> 按名称订购。 命令“ls -t”按最后时间列出项目
> 更改而不是按字母顺序。 命令 `ls -r` 列出了
> 目录的内容以相反的顺序排列。
> 当您结合使用 `-t` 和 `-r` 选项时，哪个文件最后显示？
> 提示：您可能需要使用 `-l` 选项来查看
> 最后更改日期。
>
> > ## 解决方案
> > 使用 `-rt` 时，最后列出最近更改的文件。 这
> > 对于查找您最近的编辑或检查非常有用
>> 查看是否写入了新的输出文件。
> {: .solution}
{: .challenge}

### 探索其他目录

我们不仅可以在当前工作目录上使用`ls`，
但我们可以用它来列出不同目录的内容。
让我们通过运行`ls -F Desktop`来看看我们的`Desktop`目录，
IE。，
带有 `-F` **option** 和 [**argument**][Arguments] `Desktop` 的命令 `ls`。
参数 `Desktop` 告诉 `ls`
我们想要一个列表，而不是我们当前的工作目录：

~~~
$ ls -F Desktop
~~~
{: .language-bash}

~~~
shell-lesson-data/
~~~
{: .output}

请注意，如果您当前的工作目录中不存在名为 `Desktop` 的目录，
此命令将返回错误。通常，“桌面”目录存在于您的
主目录，我们假设它是 bash shell 的当前工作目录。

您的输出应该是您的所有文件和子目录的列表
桌面目录，包括你下载的 `shell-lesson-data` 目录
[本课的准备事项]({{ page.root }}{% link setup.md %})。
在许多系统上，
命令行桌面目录与您的 GUI 桌面相同。
查看您的桌面以确认您的输出是否准确。

正如您现在所看到的，使用 bash shell 强烈依赖于以下想法：
您的文件组织在分层文件系统中。
以这种方式分层组织事物有助于我们跟踪我们的工作：
可以将数百个文件放在我们的主目录中，
就像在我们的办公桌上堆放数百张印刷文件一样，
但这是一种弄巧成拙的策略。

现在我们知道 `shell-lesson-data` 目录位于我们的桌面目录中，我们
可以做两件事。

首先我们可以看一下它的内容，使用和之前一样的策略，通过
`ls` 的目录名：

~~~
$ ls -F Desktop/shell-lesson-data
~~~
{: .language-bash}

~~~
creatures/  molecules/           notes.txt    pizza.cfg  writing/
data/       north-pacific-gyre/  numbers.txt  solar.pdf

~~~
{: .output}

其次，我们实际上可以将我们的位置更改为不同的目录，所以
我们不再位于
我们的主目录。

更改位置的命令是 `cd` 后跟
目录名称来更改我们的工作目录。
`cd` 代表“更改目录”，
这有点误导：
该命令不会更改目录；
它改变了 shell 对我们所在目录的看法。
`cd` 命令类似于在图形界面中双击文件夹以进入文件夹。

假设我们要移动到上面看到的“数据”目录。 我们可以
使用以下一系列命令到达那里：

~~~
$ cd Desktop
$ cd shell-lesson-data
$ cd data
~~~
{: .language-bash}

这些命令会将我们从主目录移动到桌面目录，然后进入
`shell-lesson-data` 目录，然后进入 `data` 目录。
你会注意到 `cd` 不打印任何东西。 这个是正常的。
许多 shell 命令在成功执行后不会向屏幕输出任何内容。
但是如果我们在它之后运行`pwd`，我们可以看到我们现在
在“/Users/nelle/Desktop/shell-lesson-data/data”中。
如果我们现在不带参数运行`ls -F`，
它列出了`/Users/nelle/Desktop/shell-lesson-data/data`的内容，
因为这就是我们现在所处的位置：

~~~
$ pwd
~~~
{: .language-bash}

~~~
/Users/nelle/Desktop/shell-lesson-data/data
~~~
{: .output}

~~~
$ ls -F
~~~
{: .language-bash}

~~~
amino-acids.txt  animals.txt  morse.txt  planets.txt  sunspot.txt
animal-counts/   elements/    pdb/       salmon.txt
~~~
{: .output}

我们现在知道如何沿着目录树向下走（即如何进入子目录），
但是我们如何向上（即我们如何离开一个目录并进入它的父目录）？
我们可以尝试以下方法：

~~~
$ cd shell-lesson-data
~~~
{: .language-bash}

~~~
-bash: cd: shell-lesson-data: No such file or directory
~~~
{: .error}

但是我们得到一个错误！为什么是这样？

到目前为止，使用我们的方法，
`cd` 只能看到当前目录中的子目录。 有
查看当前位置上方目录的不同方式； 我们开始
用最简单的。

在 shell 中有一个快捷方式可以向上移动一级目录
看起来像这样：

~~~
$ cd ..
~~~
{: .language-bash}

`..` 是一个特殊的目录名含义
"包含这个的目录",
或更简洁地说，
当前目录的**父**。
果然，
如果我们在运行 `cd ..` 后运行 `pwd`，我们会回到 `/Users/nelle/Desktop/shell-lesson-data`：

~~~
$ pwd
~~~
{: .language-bash}

~~~
/Users/nelle/Desktop/shell-lesson-data
~~~
{: .output}

当我们运行 `ls` 时，特殊目录 `..` 通常不会出现。 如果我们想要
要显示它，我们可以在 `ls -F` 中添加 `-a` 选项：

~~~
$ ls -F -a
~~~
{: .language-bash}

~~~
./   .bash_profile  data/       north-pacific-gyre/  numbers.txt  solar.pdf
../  creatures/     molecules/  notes.txt            pizza.cfg    writing/
~~~
{: .output}

`-a` 代表“全部显示”；
它强制 `ls` 向我们显示以 `.` 开头的文件和目录名称，
例如`..`（如果我们在`/Users/nelle`中，指的是`/Users`目录）。
如你看到的，
它还显示了另一个特殊的目录，它被称为`.`，
这意味着“当前工作目录”。
给它起个名字似乎是多余的，
但我们很快就会看到它的一些用途。

请注意，在大多数命令行工具中，可以组合多个选项
有一个 `-` 并且选项之间没有空格：`ls -F -a` 是
相当于`ls -Fa`。

> ## 其他隐藏文件
>
> 除了隐藏目录 `..` 和 `.`，您还可能看到一个文件
> 称为 `.bash_profile`。 该文件通常包含 shell 配置
> 设置。 您可能还会看到其他文件和目录开始
> 带有`.`。 这些通常是用于配置的文件和目录
> 计算机上的不同程序。 前缀 `.` 用于防止这些
> 配置文件在使用标准 `ls` 命令时使终端混乱
> 被使用。
{: .callout}

这三个命令是在计算机上导航文件系统的基本命令：
`pwd`、`ls` 和 `cd`。 让我们探索这些命令的一些变体。 发生什么了
如果您自己键入“cd”，而不给出
目录？

~~~
$ cd
~~~
{: .language-bash}

你怎么能检查发生了什么？ `pwd` 给了我们答案！

~~~
$ pwd
~~~
{: .language-bash}

~~~
/Users/nelle
~~~
{: .output}

事实证明，不带参数的 `cd` 会将您返回到您的主目录，
如果您迷失在自己的文件系统中，那就太好了。

让我们尝试回到之前的 `data` 目录。 上次我们用
三个命令，但我们实际上可以将目录列表串在一起
一步移动到“数据”：

~~~
$ cd Desktop/shell-lesson-data/data
~~~
{: .language-bash}

通过运行 `pwd` 和 `ls -F` 检查我们是否已移动到正确的位置。

如果我们想从数据目录上移一级，我们可以使用`cd ...`。但
还有另一种方法可以移动到任何目录，无论您的
当前位置。

到目前为止，在指定目录名称，甚至是目录路径（如上）时，
我们一直在使用**相对路径**。当您将相对路径与命令一起使用时
像`ls`或`cd`，它试图从我们所在的位置找到那个位置，
而不是从文件系统的根目录。

但是，可以通过以下方式指定目录的**绝对路径**
包括它从根目录开始的整个路径，由
前导斜线。前导 `/` 告诉计算机遵循从
文件系统的根目录，所以它总是指一个目录，
无论我们在哪里运行命令。

这允许我们从任何地方移动到我们的 `shell-lesson-data` 目录
文件系统（包括来自 `data` 的内部）。寻找绝对路径
我们正在寻找，我们可以使用 `pwd` 然后提取我们需要的部分
移动到“shell-lesson-data”。

~~~
$ pwd
~~~
{: .language-bash}

~~~
/Users/nelle/Desktop/shell-lesson-data/data
~~~
{: .output}

~~~
$ cd /Users/nelle/Desktop/shell-lesson-data
~~~
{: .language-bash}

运行 `pwd` 和 `ls -F` 以确保我们在我们期望的目录中。

> ## 另外两个快捷方式
>
> shell 解释一个波浪号 (`~`) 字符在一个路径的开头
> 表示“当前用户的主目录”。 例如，如果 Nelle 的家
> 目录为 `/Users/nelle`，则 `~/data` 等价于
> `/用户/nelle/数据`。 这仅在它是第一个字符时才有效
> 路径：`here/there/~/elsewhere` *不是* `here/there/Users/nelle/elsewhere`。
>
> 另一个快捷方式是`-`（破折号）字符。 `cd` 会将 `-` 翻译成
> *我之前所在的目录*，比记住要快，
> 然后输入完整路径。 这是一种*非常*有效的移动方式
> *在两个目录之间来回切换* -- 即如果你执行 `cd -` 两次，
> 你最终回到了起始目录。
>
> `cd ..` 和 `cd -` 的区别是
> 前者带你 *up*，而后者带你 *back*。
>
> ----
> 试试看！
> 首先导航到`~/Desktop/shell-lesson-data`（你应该已经在那里了）。
> ~~~
> $ cd ~/Desktop/shell-lesson-data
> ~~~
> {: .language-bash}
>
> 然后`cd`进入`creatures`目录
> ~~~
> $ cd creatures
> ~~~
> {: .language-bash}
>
> 现在如果你运行
> ~~~
> $ cd -
> ~~~
> {: .language-bash}
> 你会看到你回到了`~/Desktop/shell-lesson-data`。
> 再次运行 `cd -` 并返回 `~/Desktop/shell-lesson-data/creatures`
{: .callout}

> ## 绝对路径与相对路径
>
> 从`/Users/amanda/data`开始，
> Amanda 可以使用以下哪些命令导航到她的主目录，
> 哪个是`/Users/amanda`？
>
> 1. `cd .`
> 2. `cd /`
> 3. `cd /home/amanda`
> 4. `cd ../..`
> 5. `cd ~`
> 6. `cd home`
> 7. `cd ~/data/..`
> 8. `cd`
> 9. `cd ..`
>
> > ## 解决方案
> > 1.否：`.`代表当前目录。
> > 2.否：`/`代表根目录。
> > 3. 否：Amanda 的主目录是 `/Users/amanda`。
> > 4. 否：此命令上升两级，即以`/Users`结尾。
> > 5. 是：`~` 代表用户的主目录，在本例中为`/Users/amanda`。
> > 6. 否：此命令将导航到当前目录中的目录 `home`
> > 如果存在。
> > 7. 是：不必要的复杂，但正确。
> > 8. 是：返回用户主目录的快捷方式。
> > 9. 是：上升一级。
> {: .solution}
{: .challenge}

> ## 相对路径分辨率
>
> 使用下面的文件系统图，如果 `pwd` 显示 `/Users/thing`，
> `ls -F ../backup` 会显示什么？
>
> 1.  `../backup: No such file or directory`
> 2.  `2012-12-01 2013-01-08 2013-01-27`
> 3.  `2012-12-01/ 2013-01-08/ 2013-01-27/`
> 4.  `original/ pnas_final/ pnas_sub/`
>
> ![Users 目录下的目录树，其中“/Users”包含
目录“备份”和“事物”； “/Users/backup”包含“原始”，
“pnas_final”和“pnas_sub”； “/Users/thing”包含“备份”； 和
“/Users/thing/backup”包含“2012-12-01”、“2013-01-08”和
“2013-01-27”](../fig/filesystem-challenge.svg)
>
> > ## 解决方案
> > 1. 否：在`/Users`中有*有一个`backup`目录。
> > 2.否：这是`Users/thing/backup`的内容，
> > 但是使用`..`，我们要求更上一层。
> > 3. 否：见前面的解释。
> > 4. 是：`../backup/` 指的是`/Users/backup/`。
> {: .solution}
{: .challenge}

> ## `ls` 阅读理解
>
> 使用下面的文件系统图，
> 如果 `pwd` 显示 `/Users/backup`，
> 和 `-r` 告诉 `ls` 以相反的顺序显示内容，
> 哪些命令将产生以下输出：
>
> ~~~
> pnas_sub/ pnas_final/ original/
> ~~~
> {: .output}
>
> ![Users 目录下的目录树，其中“/Users”包含
目录“备份”和“事物”； “/Users/backup”包含“原始”，
“pnas_final”和“pnas_sub”； “/Users/thing”包含“备份”； 和
“/Users/thing/backup”包含“2012-12-01”、“2013-01-08”和
“2013-01-27”](../fig/filesystem-challenge.svg)
>
> 1.  `ls pwd`
> 2.  `ls -r -F`
> 3.  `ls -r -F /Users/backup`
>
> > ## 解决方案
> > 1. 否：`pwd` 不是目录名。
> > 2. 是：没有目录参数的`ls`列出文件和目录
>> 在当前目录中。
> > 3. 是：明确使用绝对路径。
> {: .solution}
{: .challenge}


## Shell命令的一般语法
我们现在遇到了命令、选项和参数，
但将一些术语形式化也许是有用的。

将以下命令视为命令的一般示例，
我们将分解成它的组成部分：

~~~
$ ls -F /
~~~
{: .language-bash}

![shell 命令的一般语法](../fig/shell_command_syntax.svg)

`ls` 是**命令**，带有一个**选项** `-F` 和一个
**参数** `/`。
我们已经遇到过选择
以单个破折号 (`-`) 或两个破折号 (`--`) 开头，
它们会改变命令的行为。
[参数] 告诉命令要操作什么（例如文件和目录）。
有时选项和参数被称为**参数**。
可以使用多个选项和多个参数调用命令，但是
命令并不总是需要参数或选项。

您有时可能会看到选项被称为 **switches** 或 **flags**，
特别是对于没有参数的选项。在本课中，我们将坚持
使用术语*选项*。

每个部分由空格分隔：如果省略空格
在 `ls` 和 `-F` 之间，shell 会寻找一个名为 `ls-F` 的命令，它
不存在。此外，大写也很重要。
例如，`ls -s` 将在名称旁边显示文件和目录的大小，
而 `ls -S` 将按大小对文件和目录进行排序，如下所示：

~~~
$ cd ~/Desktop/shell-lesson-data
$ ls -s data
~~~
{: .language-bash}
~~~
total 116
 4 amino-acids.txt   4 animals.txt   4 morse.txt  12 planets.txt  76 sunspot.txt
 4 animal-counts     4 elements      4 pdb         4 salmon.txt
~~~
{: .output}
~~~
$ ls -S data
~~~
{: .language-bash}
~~~
sunspot.txt  animal-counts  pdb        amino-acids.txt  salmon.txt
planets.txt  elements       morse.txt  animals.txt
~~~
{: .output}

把所有这些放在一起，我们上面的命令给了我们一个清单
根目录`/`中的文件和目录。
下面给出了您可能从上述命令获得的输出示例：

~~~
$ ls -F /
~~~
{: .language-bash}

~~~
Applications/         System/
Library/              Users/
Network/              Volumes/
~~~
{: .output}


### Nelle 的管道：组织文件

对文件和目录了解这么多，
Nelle 已准备好组织蛋白质分析机器将创建的文件。
第一的，
她创建了一个名为“north-pacific-gyre”的目录
（提醒自己数据来自哪里）。
里面，
她创建了一个名为“2012-07-03”的目录，
这是她开始处理样品的日期。
她过去常使用“会议论文”和“修订结果”之类的名称，
但几年后她发现它们很难理解。
（最后一根稻草是当她发现自己在创造
一个名为 `revised-revised-results-3` 的目录。）

> ## 排序输出
>
> Nelle 将她的目录命名为“年-月-日”，
> 几个月和几天的前导零，
> 因为 shell 按字母顺序显示文件和目录名称。
> 如果她使用月份名称，
> 十二月将在七月之前到来；
> 如果她不使用前导零，
> 十一月（'11'）将在七月（'7'）之前到来。 同样，将年份放在首位
> 表示 2012 年 6 月将在 2013 年 6 月之前到来。
{: .callout}

她的每个物理样本都根据她的实验室惯例进行标记
具有唯一的十个字符的 ID，
例如“NENE01729A”。
这个 ID 是她在收集日志中使用的
记录样品的位置、时间、深度和其他特征，
所以她决定将它用作每个数据文件名称的一部分。
由于分析机器的输出是纯文本，
她会将她的文件命名为“NENE01729A.txt”、“NENE01812A.txt”等。
所有 1520 个文件都将进入同一目录。

现在在她的当前目录 `shell-lesson-data` 中，
Nelle 可以使用以下命令查看她拥有的文件：

~~~
$ ls north-pacific-gyre/2012-07-03/
~~~
{: .language-bash}

这个命令要输入很多，
但她可以通过所谓的 **tab 补全** 让 shell 完成大部分工作。
如果她键入：

~~~
$ ls nor
~~~
{: .language-bash}

然后按 <kbd>Tab</kbd> （她键盘上的 Tab 键），
shell 自动为她完成目录名称：

~~~
$ ls north-pacific-gyre/
~~~
{: .language-bash}

如果她再次按 <kbd>Tab</kbd>，
Bash 会将 `2012-07-03/` 添加到命令中，
因为这是唯一可能的完成。
再次按 <kbd>Tab</kbd> 什么也不做，
因为有 19 种可能性；
按 <kbd>Tab</kbd> 两次调出所有文件的列表，
等等。
这称为 **tab 补全**，
随着我们的继续，我们将在许多其他工具中看到它。

[Arguments]: https://swcarpentry.github.io/shell-novice/reference.html#argument

{% include links.md %}
