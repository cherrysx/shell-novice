---
title: "使用文件和目录"
teaching: 30
exercises: 20
questions:
- "如何创建、复制和删除文件和目录？"
- "如何编辑文件？"
objectives:
- "创建与给定图表匹配的目录层次结构。"
- "在该层次结构中，使用编辑器或通过复制和重命名现有文件创建文件。"
- "删除、复制和移动指定的文件和/或目录。"
keypoints:
- "`cp [old] [new]` 复制一个文件。"
- "`mkdir [path]` 创建一个新目录。"
- "`mv [old] [new]` 移动（重命名）文件或目录。"
- "`rm [path]` 移除（删除）一个文件。"
- "`*` 匹配文件名中的零个或多个字符，因此 `*.txt` 匹配所有以 `.txt` 结尾的文件。"
- "`?` 匹配文件名中的任何单个字符，因此 `?.txt` 匹配 `a.txt` 但不匹配 `any.txt`。"
- "可以通过多种方式描述 Control 键的使用，包括“Ctrl-X”、“Control-X”和“^X”。"
- "Shell没有垃圾箱：一旦删除某些内容，它就真的消失了。"
- "大多数文件的名称是 `something.extension`。 扩展名不是必需的，也不保证任何东西，但通常用于指示文件中的数据类型。"
- "根据您所做的工作类型，您可能需要比 Nano 更强大的文本编辑器。"
---
## 创建目录
我们现在知道如何探索文件和目录，
但是我们如何首先创建它们？

### 第一步：看看我们在哪里以及我们已经拥有什么
让我们回到桌面上的 `shell-lesson-data` 目录
并使用 `ls -F` 查看它包含的内容：

~~~
$ pwd
~~~
{: .language-bash}

~~~
/Users/nelle/Desktop/shell-lesson-data
~~~
{: .output}

~~~
$ ls -F
~~~
{: .language-bash}

~~~
creatures/  molecules/           notes.txt    pizza.cfg  writing/
data/       north-pacific-gyre/  numbers.txt  solar.pdf
~~~
{: .output}

### 创建目录

让我们使用命令 `mkdir thesis` 创建一个名为 `thesis` 的新目录
（没有输出）：

~~~
$ mkdir thesis
~~~
{: .language-bash}

正如你可能从它的名字中猜到的那样，
`mkdir` 表示“制作目录”。
由于 `thesis` 是相对路径
（即，没有前导斜杠，例如`/what/ever/thesis`），
在当前工作目录中创建新目录：

~~~
$ ls -F
~~~
{: .language-bash}

~~~
creatures/  molecules/           notes.txt    pizza.cfg  thesis/
data/       north-pacific-gyre/  numbers.txt  solar.pdf  writing/
~~~
{: .output}

由于我们刚刚创建了 `thesis` 目录，所以里面还没有任何内容：

~~~
$ ls -F thesis
~~~
{: .language-bash}

请注意，`mkdir` 不限于一次创建一个目录。
`-p` 选项允许 `mkdir` 创建具有嵌套子目录的目录
在一次操作中：

~~~
$ mkdir -p project/data project/results
~~~
{: .language-bash}

`ls` 命令的`-R` 选项将列出目录中的所有嵌套子目录。
让我们使用 `ls -FR` 递归地列出我们刚刚在
`项目`目录：

~~~
$ ls -FR project
~~~
{: .language-bash}

~~~
project/:
data/  results/

project/data:

project/results:
~~~
{: .output}

> ## 做同一件事的两种方法
> 使用 shell 创建目录与使用文件资源管理器没有什么不同。
> 如果您使用操作系统的图形文件资源管理器打开当前目录，
> `thesis` 目录也会出现在那里。
> 虽然 shell 和文件资源管理器是与文件交互的两种不同方式，
> 文件和目录本身是相同的。
{: .callout}

> ## 文件和目录的好名字
>
> 复杂的文件名和目录名会让你的生活变得痛苦
> 在命令行上工作时。 在这里我们提供一些有用的
> 文件和目录名称的提示。
>
> 1. 不要用空格
>
>    空格可以让名字更有意义，
>    但是由于空格用于分隔命令行上的参数
>    最好在文件名和目录名中避免使用它们。
>    您可以使用 `-` 或 `_` 代替（例如 `north-pacific-gyre/` 而不是 `north pacific gyre/`）。
>    要对此进行测试，请尝试输入“mkdir north pacific gyre”并查看是哪个目录（或多个目录！）
>    是在您使用 `ls -F` 检查时生成的。
>
> 2. 名称不要以 `-`（破折号）开始。
>
>    命令将以 `-` 开头的名称视为选项。
>
> 3. 坚持使用字母、数字、`.`（句点或“句号”）、`-`（破折号）和`_`（下划线）。
>
>    许多其他字符在命令行中具有特殊含义。
>    我们将在本课中了解其中的一些内容。
>    有一些特殊字符会导致您的命令无法正常工作
>    预期的，甚至可能导致数据丢失。
>
> 如果您需要引用包含空格的文件或目录的名称
> 或其他特殊字符，您应该用引号 (`""`) 将名称括起来。
{: .callout}

### 创建一个文本文件
让我们使用 `cd` 将我们的工作目录更改为 `thesis`，
然后运行一个名为 Nano 的文本编辑器来创建一个名为 `draft.txt` 的文件：

~~~
$ cd thesis
$ nano draft.txt
~~~
{: .language-bash}

> ## 哪个编辑器？
>
> 当我们说“nano”是一个文本编辑器时，我们的意思是“文本”：它可以
> 仅适用于纯字符数据，不适用于表格、图像或任何其他
> 人性化媒体。我们在示例中使用它是因为它是
> 最简单的文本编辑器。然而，由于这种特性，它可能
> 对你需要做的工作不够强大或不够灵活
> 在本次研讨会之后。在 Unix 系统（如 Linux 和 macOS）上，
> 许多程序员使用 [Emacs](http://www.gnu.org/software/emacs/) 或
> [Vim](http://www.vim.org/)（两者都需要更多时间学习），
> 或图形编辑器，例如
> [Gedit](http://projects.gnome.org/gedit/)。在 Windows 上，您可能希望
> 使用 [Notepad++](http://notepad-plus-plus.org/)。 Windows 也有一个内置的
> 名为“记事本”的编辑器，可以在同一命令行中运行
> 在本课中使用“nano”的方式。
>
> 无论您使用什么编辑器，您都需要知道它在哪里搜索
> 用于和保存文件。如果你从 shell 启动它，它会（可能）
> 使用您当前的工作目录作为其默认位置。如果你使用
> 您计算机的开始菜单，它可能希望将文件保存在您的桌面或
> 文档目录。您可以通过导航到
> 第一次“另存为...”时的另一个目录
{: .callout}

让我们输入几行文本。
一旦我们对文本感到满意，我们可以按 <kbd>Ctrl</kbd>+<kbd>O</kbd>
（按 <kbd>Ctrl</kbd> 或 <kbd>Control</kbd> 键，同时
按住它，按 <kbd>O</kbd> 键）将我们的数据写入磁盘
（系统会询问我们要将其保存到哪个文件：
按 <kbd>Return</kbd> 接受建议的默认值 `draft.txt`）。

<div style="width:80%; margin: auto;"><img alt="screenshot of nano text editor in action"
src="../fig/nano-screenshot.png"></div>

保存文件后，我们可以使用 <kbd>Ctrl</kbd>+<kbd>X</kbd> 退出编辑器并
返回shell。

> ## 控制、Ctrl 或 ^ 键
>
> Control 键也称为“Ctrl”键。 有多种方式
> 其中可能会描述使用 Control 键。 例如，您可以
> 查看按 <kbd>Control</kbd> 键的说明，并在按住它的同时，
> 按 <kbd>X</kbd> 键，描述为：
>
> * `Control-X`
> * `Control+X`
> * `Ctrl-X`
> * `Ctrl+X`
> * `^X`
> * `C-x`
>
> 在 nano 中，您会在屏幕底部看到 `^G Get Help ^O WriteOut`。
> 这意味着您可以使用 `Control-G` 来获取帮助并使用 `Control-O` 来保存您的
> 文件。
{: .callout}

`nano` 退出后不会在屏幕上留下任何输出，
但是`ls`现在显示我们已经创建了一个名为`draft.txt`的文件：

~~~
$ ls
~~~
{: .language-bash}

~~~
draft.txt
~~~
{: .output}

> ## 以不同的方式创建文件
>
> 我们已经了解了如何使用 `nano` 编辑器创建文本文件。
> 现在，尝试以下命令：
>
> ~~~
> $ touch my_file.txt
> ~~~
> {: .language-bash}
>
> 1.  `touch` 命令做了什么？
>     当您使用 GUI 文件资源管理器查看当前目录时，
>     文件是否显示？
>
> 2.  使用 `ls -l` 检查文件。 `my_file.txt` 有多大？
>
> 3.  您什么时候想以这种方式创建文件？
>
> > ## 解决方案
> > 1.  `touch` 命令生成一个名为 `my_file.txt` 的新文件
> >     你的当前目录。 你
> >     可以通过在
> >     命令行提示符。 `my_file.txt` 也可以在您的
> >     GUI 文件浏览器。
> >
> > 2.  当您使用 `ls -l` 检查文件时，请注意
> >     `my_file.txt` 为 0 字节。 换句话说，它不包含任何数据。
> >     如果您使用文本编辑器打开 `my_file.txt`，它是空白的。
> >
> > 3.  有些程序本身不生成输出文件，但
> >     而是要求已经生成了空文件。
> >     当程序运行时，它会搜索现有文件以
> >     填充其输出。 触摸命令允许您
> >     有效地生成一个空白文本文件供此类使用
> >     节目。
> {: .solution}
{: .challenge}

> ## 名字里有什么？
>
> 你可能已经注意到 Nelle 的所有文件都被命名为“something dot”
> something'，在这部分课程中，我们总是使用扩展名
> `.txt`。这只是一个约定：我们可以将文件称为“mythesis”或
> 几乎任何我们想要的东西。但是，大多数人使用两部分名称
> 大多数时候帮助他们（和他们的程序）告诉不同的种类
> 文件分开。这种名称的第二部分称为
> **文件扩展名**并表示
> 文件包含什么类型的数据：`.txt` 表示纯文本文件，`.pdf`
> 表示一个PDF文件，`.cfg`是一个配置文件，里面有很多参数
> 对于某些程序或其他程序，`.png` 是 PNG 图像，依此类推。
>
> 这只是一个约定，尽管它很重要。文件包含
> 字节：由我们和我们的程序来解释这些字节
> 按规则对纯文本文件、PDF文件、配置
> 文件、图像等。
>
> 将鲸鱼的 PNG 图像命名为 `whale.mp3`
> 神奇地把它变成鲸歌的录音，虽然它*可能*
> 导致操作系统尝试使用音乐播放器打开它
> 当有人双击它时。
{: .callout}

## 移动文件和目录
返回到 `shell-lesson-data` 目录，

```
$ cd ~/Desktop/shell-lesson-data/
```
{: .language-bash}

I在我们的 `thesis` 目录中，我们有一个文件 `draft.txt`
这不是一个特别有用的名字，
所以让我们使用 `mv` 更改文件名，
这是“move”的缩写：

~~~
$ mv thesis/draft.txt thesis/quotes.txt
~~~
{: .language-bash}

第一个参数告诉 `mv` 我们正在“move”什么，
而第二个是它要去的地方。
在这种情况下，
我们将 `thesis/draft.txt` 移动到 `thesis/quotes.txt`，
这与重命名文件具有相同的效果。
果然，
`ls` 向我们展示了 `thesis` 现在包含一个名为 `quotes.txt` 的文件：

~~~
$ ls thesis
~~~
{: .language-bash}

~~~
quotes.txt
~~~
{: .output}

指定目标文件名时必须小心，因为 `mv` 会
默默地覆盖任何现有的同名文件，这可能
导致数据丢失。 一个附加选项，`mv -i`（或 `mv --interactive`），
可用于让 `mv` 在覆盖前要求您确认。

请注意，`mv` 也适用于目录。

让我们将 `quotes.txt` 移动到当前工作目录中。
我们再次使用`mv`，
但是这次我们将只使用目录的名称作为第二个参数
告诉 `mv` 我们要保留文件名
但将文件放在新的地方。
（这就是该命令被称为“移动”的原因。）
在这种情况下，
我们使用的目录名是我们前面提到的特殊目录名`.`。

~~~
$ mv thesis/quotes.txt .
~~~
{: .language-bash}

效果是将文件从它所在的目录移动到当前工作目录。
`ls` 现在告诉我们`thesis` 是空的：

~~~
$ ls thesis
~~~
{: .language-bash}

~~~
$
~~~
{: .output}

或者，我们可以确认文件 `quotes.txt` 不再存在于 `thesis` 目录中
通过明确尝试列出它：

~~~
$ ls thesis/quotes.txt
~~~
{: .language-bash}

```
ls: cannot access 'thesis/quotes.txt': No such file or directory
```
{: .error}

以文件名或目录作为参数的 `ls` 仅列出请求的文件或目录。
如果作为参数给出的文件不存在，shell 会返回一个错误，如我们上面所见。
我们可以使用它来查看 `quotes.txt` 现在存在于我们的当前目录中：

~~~
$ ls quotes.txt
~~~
{: .language-bash}

~~~
quotes.txt
~~~
{: .output}

> ## 将文件移动到新文件夹
>
> 运行以下命令后，
> Jamie意识到她将文件 `sucrose.dat` 和 `maltose.dat` 放入了错误的文件夹。
> 文件应该已放置在 `raw` 文件夹中。
>
> ~~~
> $ ls -F
>  analyzed/ raw/
> $ ls -F analyzed
> fructose.dat glucose.dat maltose.dat sucrose.dat
> $ cd analyzed
> ~~~
> {: .language-bash}
>
> 填空以将这些文件移动到 `raw/` 文件夹
>（即她忘记放入的那个）
>
> ~~~
> $ mv sucrose.dat maltose.dat ____/____
> ~~~
> {: .language-bash}
> > ## Solution
> > ```
> > $ mv sucrose.dat maltose.dat ../raw
> > ```
> > {: .language-bash}
>> 回想一下，`..` 指的是父目录（即当前目录之上的一个）
> > 并且那个 `.` 指的是当前目录。
> {: .solution}
{: .challenge}

## 复制文件和目录

`cp` 命令的工作方式非常类似于 `mv`，
除了它复制文件而不是移动它。
我们可以使用`ls`检查它是否正确
以两条路径作为参数——像大多数 Unix 命令一样，
`ls` 可以一次给定多个路径：

~~~
$ cp quotes.txt thesis/quotations.txt
$ ls quotes.txt thesis/quotations.txt
~~~
{: .language-bash}

~~~
quotes.txt   thesis/quotations.txt
~~~
{: .output}

我们还可以使用
[递归]（https://en.wikipedia.org/wiki/Recursion）选项`-r`，
例如备份目录：

```
$ cp -r thesis thesis_backup
```
{: .language-bash}

我们可以通过列出 `thesis` 和 `thesis_backup` 目录的内容来检查结果：

```
$ ls thesis thesis_backup
```
{: .language-bash}

```
thesis:
quotations.txt

thesis_backup:
quotations.txt
```
{: .output}


> ## 重命名文件
>
> 假设您在当前目录中创建了一个纯文本文件以包含
> 分析数据需要进行的统计测试，并将其命名为：`statstics.txt`
>
> 创建并保存此文件后，您意识到您拼错了文件名！ 你想要
> 纠正错误，您可以使用以下哪个命令来纠正错误？
>
> 1. `cp statstics.txt statistics.txt`
> 2. `mv statstics.txt statistics.txt`
> 3. `mv statstics.txt .`
> 4. `cp statstics.txt .`
>
> > ## 解决方案
> > 1. 不。虽然这会创建一个具有正确名称的文件，
> >    错误命名的文件仍然存在于目录中
> >    并且需要删除。
> > 2. 是的，这可以重命名文件。
> > 3. 不，句点（.）表示将文件移动到哪里，但不提供新的文件名；
> >    相同的文件名
> >    无法创建。
> > 4. 不，句点（.）表示将文件复制到哪里，但不提供新的文件名；
> >    无法创建相同的文件名。
> {: .solution}
{: .challenge}

> ## 移动和复制
>
> 关闭 `ls` 命令的输出如下所示的顺序是什么？
>
> ~~~
> $ pwd
> ~~~
> {: .language-bash}
> ~~~
> /Users/jamie/data
> ~~~
> {: .output}
> ~~~
> $ ls
> ~~~
> {: .language-bash}
> ~~~
> proteins.dat
> ~~~
> {: .output}
> ~~~
> $ mkdir recombined
> $ mv proteins.dat recombined/
> $ cp recombined/proteins.dat ../proteins-saved.dat
> $ ls
> ~~~
> {: .language-bash}
>
>
> 1.   `proteins-saved.dat recombined`
> 2.   `recombined`
> 3.   `proteins.dat recombined`
> 4.   `proteins-saved.dat`
>
> > ## 解决方案
> > 我们从 `/Users/jamie/data` 目录开始，并创建一个名为 `recombined` 的新文件夹。
> > 第二行将文件 `proteins.dat` 移动 (`mv`) 到新文件夹 (`recombined`)。
> > 第三行复制了我们刚刚移动的文件。
> > 这里棘手的部分是文件被复制到的位置。
> > 回想一下，`..` 的意思是“上一级”，所以复制的文件现在位于 `/Users/jamie` 中。
> > 请注意，`..` 是相对于当前工作进行解释的
> > 目录，**不是**相对于被复制文件的位置。
> > 所以，唯一会使用 ls 显示的东西（在 `/Users/jamie/data` 中）是重新组合的文件夹。
> >
> > 1. 不，见上面的解释。 `proteins-saved.dat` 位于 `/Users/jamie`
> > 2. 是的
> > 3. 不，见上面的解释。 `proteins.dat` 位于 `/Users/jamie/data/recombined`
> > 4. 不，见上面的解释。 `proteins-saved.dat` 位于 `/Users/jamie`
> {: .solution}
{: .challenge}

## 删除文件和目录

返回到 `shell-lesson-data` 目录，
让我们通过删除我们创建的 `quotes.txt` 文件来整理这个目录。
我们将使用的 Linux 命令是“rm”（“remove”的缩写）：

~~~
$ rm quotes.txt
~~~
{: .language-bash}

We can confirm the file has gone using `ls`:

~~~
$ ls quotes.txt
~~~
{: .language-bash}

```
ls: cannot access 'quotes.txt': No such file or directory
```
{: .error}

> ## 删除是永远的
>
> Linux shell 没有我们可以恢复的垃圾箱已删除
> 文件来自（尽管大多数 Unix 的图形界面都这样做）。 反而，
> 当我们删除文件时，它们会从文件系统中取消链接，这样
> 他们在磁盘上的存储空间是可以回收的。 查找工具和
> 恢复已删除文件确实存在，但不能保证它们会
> 在任何特定情况下工作，因为计算机可能会回收
> 文件的磁盘空间。
{: .callout}


> ## 安全地使用 rm
>
> 当我们执行 `rm -i thesis_backup/quotations.txt` 时会发生什么？
> 为什么我们在使用 `rm` 时需要这种保护？
>
> > ## 解决方案
> > ```
> > rm: remove regular file 'thesis_backup/quotations.txt'? y
> > ```
> > {: .output}
> > `-i` 选项将在（每次）删除之前提示（使用 <kbd>Y</kbd> 确认删除
> > 或 <kbd>N</kbd> 保留文件）。
> > Linux shell 没有垃圾箱，所以所有被删除的文件都会永远消失。
> > 通过使用 `-i` 选项，我们有机会检查我们是否只删除文件
> > 我们要删除的。
> {: .solution}
{: .challenge}


如果我们尝试使用 `rm thesis` 删除 `thesis` 目录，
我们收到一条错误消息：

~~~
$ rm thesis
~~~
{: .language-bash}

~~~
rm: cannot remove `thesis': Is a directory
~~~
{: .error}

发生这种情况是因为 `rm` 默认情况下仅适用于文件，而不适用于目录。

`rm` 可以删除目录*及其所有内容*，如果我们使用
递归选项`-r`，它会这样做*没有任何确认提示*：

~~~
$ rm -r thesis
~~~
{: .language-bash}

鉴于无法检索使用 shell 删除的文件，
`rm -r` *应谨慎使用*
（您可以考虑添加交互式选项 `rm -r -i`）。

## 多个文件和目录的操作

通常需要一次复制或移动多个文件。
这可以通过提供单个文件名列表来完成，
或使用通配符指定命名模式。

> ## 使用多个文件名复制
>
> 对于本练习，您可以测试 `shell-lesson-data/data` 目录中的命令。
>
> 在下面的例子中，当给定多个文件名和一个目录名时，`cp` 会做什么？
>
> ~~~
> $ mkdir backup
> $ cp amino-acids.txt animals.txt backup/
> ~~~
> {: .language-bash}
>
> 在下面的示例中，当给定三个或更多文件名时，`cp` 会做什么？
>
> ~~~
> $ ls -F
> ~~~
> {: .language-bash}
> ~~~
> amino-acids.txt  animals.txt  elements/  pdb/         salmon.txt
> animal-counts/   backup/      morse.txt  planets.txt  sunspot.txt
> ~~~
> {: .output}
> ~~~
> $ cp amino-acids.txt animals.txt morse.txt
> ~~~
> {: .language-bash}
>
> > ## 解决方案
> > 如果给出多个文件名后跟一个目录名
> >（即目标目录必须是最后一个参数），
> > `cp` 将文件复制到指定目录。
> >
> > 如果给定三个文件名，`cp` 会抛出如下错误，
> > 因为它需要一个目录名作为最后一个参数。
> >
> > ```
> > cp: target 'morse.txt' is not a directory
> > ```
> > {: .error}
> {: .solution}
{: .challenge}

### 使用通配符一次访问多个文件

> ## 通配符
>
> `*` 是一个**通配符**，它匹配零个或多个字符。
> 让我们考虑 `shell-lesson-data/molecules` 目录：
> `*.pdb` 匹配 `ethane.pdb`、`propane.pdb` 和每个
> 以“.pdb”结尾的文件。另一方面，`p*.pdb` 只匹配
> `pentane.pdb` 和 `propane.pdb`，因为只有前面的 'p'
> 匹配以字母“p”开头的文件名。
>
> `?` 也是一个通配符，但它只匹配一个字符。
> 所以 `?ethane.pdb` 会匹配 `methane.pdb` 而
> `*ethane.pdb` 匹配 `ethane.pdb` 和 `methane.pdb`。
>
> 通配符可以相互结合使用
> 例如`???ane.pdb` 匹配后跟 `ane.pdb` 的三个字符，
> 给予 `cubane.pdb ethane.pdb octane.pdb`。
>
> 当 shell 看到通配符时，它会扩展通配符以创建一个
> 匹配文件名列表*在*运行之前的命令之前
> 要求。作为一个例外，如果通配符表达式不匹配
> 任何文件，Bash 会将表达式作为参数传递给命令
> 原样。例如，在 `molecules` 目录中输入 `ls *.pdf`
> （仅包含名称以 `.pdb` 结尾的文件）导致
> 没有名为 `*.pdf` 的文件的错误消息。
> 但是，通常像 `wc` 和 `ls` 这样的命令会看到
> 匹配这些表达式的文件名，但不匹配通配符
> 他们自己。处理的是shell程序，而不是其他程序
> 扩展通配符。
{: .callout}

> ## 列出匹配模式的文件名
>
> 在 `molecules` 目录中运行时，`ls` 命令将
> 产生这个输出？
>
> `ethane.pdb   methane.pdb`
>
> 1. `ls *t*ane.pdb`
> 2. `ls *t?ne.*`
> 3. `ls *t??ne.pdb`
> 4. `ls ethane.*`
>
> > ## 解决方案
> > 解决方案是`3.`
> >
> > `1.` 显示名称中包含零个或多个字符 (`*`) 的所有文件
> > 后跟字母 `t`,
> > 然后是零个或多个字符 (`*`)，后跟 `ane.pdb`。
> > 这给出了`ethane.pdb methane.pdb octane.pdb pentane.pdb`。
> >
> > `2.` 显示名称以零个或多个字符 (`*`) 开头的所有文件，后跟
> > 字母 `t`,
> > 然后是单个字符 (`?`)，然后是 `ne.`，后跟零个或多个字符 (`*`)。
> > 这将为我们提供 `octane.pdb` 和 `pentane.pdb` 但不匹配任何内容
> > 以 `thane.pdb` 结尾。
> >
> > `3.`通过匹配`t`和`ne`之间的两个字符（`??`）修复了选项2的问题。
> > 这就是解决方案。
> >
> > `4.` 只显示以 `ethane.` 开头的文件。
> {: .solution}
{: .challenge}

> ## 更多关于通配符
>
> Sam 有一个目录，其中包含校准数据、数据集和描述
> 数据集：
>
> ~~~
> .
> ├── 2015-10-23-calibration.txt
> ├── 2015-10-23-dataset1.txt
> ├── 2015-10-23-dataset2.txt
> ├── 2015-10-23-dataset_overview.txt
> ├── 2015-10-26-calibration.txt
> ├── 2015-10-26-dataset1.txt
> ├── 2015-10-26-dataset2.txt
> ├── 2015-10-26-dataset_overview.txt
> ├── 2015-11-23-calibration.txt
> ├── 2015-11-23-dataset1.txt
> ├── 2015-11-23-dataset2.txt
> ├── 2015-11-23-dataset_overview.txt
> ├── backup
> │   ├── calibration
> │   └── datasets
> └── send_to_bob
>     ├── all_datasets_created_on_a_23rd
>     └── all_november_files
> ~~~
> {: .language-bash}
>
> 在开始另一次实地考察之前，她想备份她的数据和
> 将一些数据集发送给她的同事 Bob。 Sam 使用以下命令
> 完成工作：
>
> ~~~
> $ cp *dataset* backup/datasets
> $ cp ____calibration____ backup/calibration
> $ cp 2015-____-____ send_to_bob/all_november_files/
> $ cp ____ send_to_bob/all_datasets_created_on_a_23rd/
> ~~~
> {: .language-bash}
>
> 帮助山姆填空。
>
> 生成的目录结构应如下所示
> ```
> .
> ├── 2015-10-23-calibration.txt
> ├── 2015-10-23-dataset1.txt
> ├── 2015-10-23-dataset2.txt
> ├── 2015-10-23-dataset_overview.txt
> ├── 2015-10-26-calibration.txt
> ├── 2015-10-26-dataset1.txt
> ├── 2015-10-26-dataset2.txt
> ├── 2015-10-26-dataset_overview.txt
> ├── 2015-11-23-calibration.txt
> ├── 2015-11-23-dataset1.txt
> ├── 2015-11-23-dataset2.txt
> ├── 2015-11-23-dataset_overview.txt
> ├── backup
> │   ├── calibration
> │   │   ├── 2015-10-23-calibration.txt
> │   │   ├── 2015-10-26-calibration.txt
> │   │   └── 2015-11-23-calibration.txt
> │   └── datasets
> │       ├── 2015-10-23-dataset1.txt
> │       ├── 2015-10-23-dataset2.txt
> │       ├── 2015-10-23-dataset_overview.txt
> │       ├── 2015-10-26-dataset1.txt
> │       ├── 2015-10-26-dataset2.txt
> │       ├── 2015-10-26-dataset_overview.txt
> │       ├── 2015-11-23-dataset1.txt
> │       ├── 2015-11-23-dataset2.txt
> │       └── 2015-11-23-dataset_overview.txt
> └── send_to_bob
>     ├── all_datasets_created_on_a_23rd
>     │   ├── 2015-10-23-dataset1.txt
>     │   ├── 2015-10-23-dataset2.txt
>     │   ├── 2015-10-23-dataset_overview.txt
>     │   ├── 2015-11-23-dataset1.txt
>     │   ├── 2015-11-23-dataset2.txt
>     │   └── 2015-11-23-dataset_overview.txt
>     └── all_november_files
>         ├── 2015-11-23-calibration.txt
>         ├── 2015-11-23-dataset1.txt
>         ├── 2015-11-23-dataset2.txt
>         └── 2015-11-23-dataset_overview.txt
> ```
> {: .language-bash}
>
> > ## 解决方案
> > ```
> > $ cp *calibration.txt backup/calibration
> > $ cp 2015-11-* send_to_bob/all_november_files/
> > $ cp *-23-dataset* send_to_bob/all_datasets_created_on_a_23rd/
> > ```
> > {: .language-bash}
> {: .solution}
{: .challenge}

> ## 组织目录和文件
>
> Jamie 正在处理一个项目，她发现她的文件不太好
> 组织：
>
> ~~~
> $ ls -F
> ~~~
> {: .language-bash}
> ~~~
> analyzed/  fructose.dat    raw/   sucrose.dat
> ~~~
> {: .output}
>
> `fructose.dat` 和 `sucrose.dat` 文件包含她的数据输出
> 分析。 她需要运行本课中涵盖的哪些命令
> 以便下面的命令将产生所示的输出？
>
> ~~~
> $ ls -F
> ~~~
> {: .language-bash}
> ~~~
> analyzed/   raw/
> ~~~
> {: .output}
> ~~~
> $ ls analyzed
> ~~~
> {: .language-bash}
> ~~~
> fructose.dat    sucrose.dat
> ~~~
> {: .output}
>
> > ## Solution
> > ```
> > mv *.dat analyzed
> > ```
> > {: .language-bash}
> > Jamie 需要将她的文件 `fructose.dat` 和 `sucrose.dat` 移动到 `analyzed` 目录。
> > shell 将扩展 *.dat 以匹配当前目录中的所有 .dat 文件。
> > `mv` 命令然后将 .dat 文件列表移动到 'analyzed' 目录。
> {: .solution}
{: .challenge}

> ## 重现文件夹结构
>
> 您正在开始一项新实验并希望复制目录
> 之前实验的结构，以便您可以添加新数据。
>
> 假设之前的实验在一个名为`2016-05-18`的文件夹中，
> 其中包含一个 `data` 文件夹，该文件夹又包含名为 `raw` 的文件夹和
> 包含数据文件的“已处理”。 目标是复制文件夹结构
> 将 `2016-05-18` 文件夹放入名为 `2016-05-20` 的文件夹中
> 使您的最终目录结构如下所示：
>
> ~~~
> 2016-05-20/
> └── data
>    ├── processed
>    └── raw
> ~~~
> {: .output}
>
> 以下哪一组命令可以实现此目标？
> 其他命令会做什么？
>
> ~~~
> $ mkdir 2016-05-20
> $ mkdir 2016-05-20/data
> $ mkdir 2016-05-20/data/processed
> $ mkdir 2016-05-20/data/raw
> ~~~
> {: .language-bash}
> ~~~
> $ mkdir 2016-05-20
> $ cd 2016-05-20
> $ mkdir data
> $ cd data
> $ mkdir raw processed
> ~~~
> {: .language-bash}
> ~~~
> $ mkdir 2016-05-20/data/raw
> $ mkdir 2016-05-20/data/processed
> ~~~
> {: .language-bash}
> ~~~
> $ mkdir -p 2016-05-20/data/raw
> $ mkdir -p 2016-05-20/data/processed
> ~~~
> {: .language-bash}
> ~~~
> $ mkdir 2016-05-20
> $ cd 2016-05-20
> $ mkdir data
> $ mkdir raw processed
> ~~~
> {: .language-bash}
> >
> > ## 解决方案
> > 前两组命令实现了这个目标。
> > 第一套使用相对路径创建之前的顶级目录
> > 子目录。
> >
> > 第三组命令会报错，因为 `mkdir` 的默认行为
> > 不会创建不存在目录的子目录：
> > 必须首先创建中级文件夹。
> >
> > 第四组命令实现了这个目标。 请记住，`-p` 选项，
> > 后跟一条或多条路径
> > 目录，将导致 `mkdir` 根据需要创建任何中间子目录。
> >
> > 最后一组命令在同一级别生成'raw'和'processed'目录
> > 作为“数据”目录。
> {: .solution}
{: .challenge}

{% include links.md %}
