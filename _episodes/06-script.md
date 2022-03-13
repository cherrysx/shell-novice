---
title: "Shell脚本"
teaching: 30
exercises: 15
questions:
- "如何保存和重复使用命令？"
objectives:
- "编写一个shell脚本，为一组固定的文件运行一个命令或一系列命令。"
- "从命令行运行shell脚本。"
- "编写一个shell脚本，对用户在命令行中定义的一组文件进行操作。"
- "创建包含您和其他人编写的shell脚本的管道。"
keypoints:
- "将命令保存在文件中（通常称为 shell 脚本）以供重复使用。"
- "`bash [filename]` 运行保存在文件中的命令。"
- "`$@` 指代所有 shell 脚本的命令行参数。"
- "`$1`、`$2` 等，指的是第一个命令行参数、第二个命令行参数等。"
- "如果值中可能包含空格，请将变量放在引号中。"
- "让用户决定处理哪些文件更灵活，更符合内置的 Unix 命令。"
---

我们终于准备好看看是什么让 shell 成为如此强大的编程环境。
我们将采用我们经常重复的命令并将它们保存在文件中
以便我们稍后可以通过键入单个命令再次重新运行所有这些操作。
由于历史原因，
保存在文件中的一堆命令通常称为 **shell 脚本**，
但不要搞错：
这些实际上是小程序。

编写 shell 脚本不仅会让你的工作更快——
您不必一遍又一遍地重新键入相同的命令 ---
它还将使其更准确（拼写错误的机会更少）和更可重复。
如果您稍后回到您的工作中（或者如果其他人找到您的工作并想在此基础上继续工作）
您只需运行脚本即可重现相同的结果，
而不必记住或重新输入一长串命令。

让我们先回到 `molecules/` 并创建一个新文件 `middle.sh`
成为我们的shell脚本：

~~~
$ cd molecules
$ nano middle.sh
~~~
{: .language-bash}

命令“nano middle.sh”在文本编辑器“nano”中打开文件“middle.sh”
（在外壳中运行）。
如果文件不存在，则会创建它。
我们可以使用文本编辑器直接编辑文件——我们只需插入以下行：

~~~
head -n 15 octane.pdb | tail -n 5
~~~
{: .source}

这是我们之前构建的管道的一种变体：
它选择文件`octane.pdb`的第11-15行。
请记住，我们*还没有*将它作为命令运行：
我们将命令放在一个文件中。

然后我们保存文件（nano 中的`Ctrl-O`），
  并退出文本编辑器（nano 中的“Ctrl-X”）。
检查目录 `molecules` 现在是否包含一个名为 `middle.sh` 的文件。

一旦我们保存了文件，
我们可以要求 shell 执行它包含的命令。
我们的 shell 被称为 `bash`，所以我们运行以下命令：

~~~
$ bash middle.sh
~~~
{: .language-bash}

~~~
ATOM      9  H           1      -4.502   0.681   0.785  1.00  0.00
ATOM     10  H           1      -5.254  -0.243  -0.537  1.00  0.00
ATOM     11  H           1      -4.357   1.252  -0.895  1.00  0.00
ATOM     12  H           1      -3.009  -0.741  -1.467  1.00  0.00
ATOM     13  H           1      -3.172  -1.337   0.206  1.00  0.00
~~~
{: .output}

果然，
如果我们直接运行该管道，我们的脚本的输出正是我们想得到的。

> ## 文本与任何内容
>
> 我们通常将 Microsoft Word 或 LibreOffice Writer 等程序称为“文本”
> editors”，但在涉及到
> 编程。 默认情况下，Microsoft Word 使用 `.docx` 文件来存储不
> 只有文本，还有关于字体、标题等的格式信息
> 开。 这些额外信息不存储为字符，并不意味着
> 像`head`这样的工具：他们希望输入文件包含
> 只有标准计算机上的字母、数字和标点符号
> 键盘。 因此，在编辑程序时，您必须使用普通的
> 文本编辑器，或小心将文件保存为纯文本。
{: .callout}

如果我们想从任意文件中选择行怎么办？
我们可以每次编辑 `middle.sh` 来更改文件名，
但这可能比再次输入命令需要更长的时间
在 shell 中并使用新的文件名执行它。
相反，让我们编辑 `middle.sh` 并使其更加通用：

~~~
$ nano middle.sh
~~~
{: .language-bash}

现在，在“nano”中，用名为“$1”的特殊变量替换文本“octane.pdb”：

~~~
head -n 15 "$1" | tail -n 5
~~~
{: .source}

在 shell 脚本中，
`$1` 表示“命令行上的第一个文件名（或其他参数）”。
我们现在可以像这样运行我们的脚本：

~~~
$ bash middle.sh octane.pdb
~~~
{: .language-bash}

~~~
ATOM      9  H           1      -4.502   0.681   0.785  1.00  0.00
ATOM     10  H           1      -5.254  -0.243  -0.537  1.00  0.00
ATOM     11  H           1      -4.357   1.252  -0.895  1.00  0.00
ATOM     12  H           1      -3.009  -0.741  -1.467  1.00  0.00
ATOM     13  H           1      -3.172  -1.337   0.206  1.00  0.00
~~~
{: .output}

或在这样的不同文件上：

~~~
$ bash middle.sh pentane.pdb
~~~
{: .language-bash}

~~~
ATOM      9  H           1       1.324   0.350  -1.332  1.00  0.00
ATOM     10  H           1       1.271   1.378   0.122  1.00  0.00
ATOM     11  H           1      -0.074  -0.384   1.288  1.00  0.00
ATOM     12  H           1      -0.048  -1.362  -0.205  1.00  0.00
ATOM     13  H           1      -1.183   0.500  -1.412  1.00  0.00
~~~
{: .output}

> ## 参数周围的双引号
>
> 出于同样的原因，我们将循环变量放在双引号内，
> 如果文件名恰好包含任何空格，
> 我们用双引号将 `$1` 括起来。
{: .callout}

目前，我们每次要调整范围时都需要编辑`middle.sh`
返回的行。
让我们通过将脚本配置为使用三个命令行参数来解决这个问题。
在第一个命令行参数 (`$1`) 之后，我们添加的每个附加参数
provide 可以通过特殊变量 `$1`, `$2`, `$3`,
分别指第一个、第二个、第三个命令行参数。

知道了这一点，我们可以使用额外的参数来定义行的范围
分别传递给`head`和`tail`：

~~~
$ nano middle.sh
~~~
{: .language-bash}

~~~
head -n "$2" "$1" | tail -n "$3"
~~~
{: .source}

我们现在可以运行：

~~~
$ bash middle.sh pentane.pdb 15 5
~~~
{: .language-bash}

~~~
ATOM      9  H           1       1.324   0.350  -1.332  1.00  0.00
ATOM     10  H           1       1.271   1.378   0.122  1.00  0.00
ATOM     11  H           1      -0.074  -0.384   1.288  1.00  0.00
ATOM     12  H           1      -0.048  -1.362  -0.205  1.00  0.00
ATOM     13  H           1      -1.183   0.500  -1.412  1.00  0.00
~~~
{: .output}

通过更改命令的参数，我们可以更改脚本的
行为：

~~~
$ bash middle.sh pentane.pdb 20 5
~~~
{: .language-bash}

~~~
ATOM     14  H           1      -1.259   1.420   0.112  1.00  0.00
ATOM     15  H           1      -2.608  -0.407   1.130  1.00  0.00
ATOM     16  H           1      -2.540  -1.303  -0.404  1.00  0.00
ATOM     17  H           1      -3.393   0.254  -0.321  1.00  0.00
TER      18              1
~~~
{: .output}

这行得通，
但是下一个阅读 `middle.sh` 的人可能需要片刻才能弄清楚它的作用。
我们可以通过在顶部添加一些 **comments** 来改进我们的脚本：

~~~
$ nano middle.sh
~~~
{: .language-bash}

~~~
# Select lines from the middle of a file.
# Usage: bash middle.sh filename end_line num_lines
head -n "$2" "$1" | tail -n "$3"
~~~
{: .source}

注释以 `#` 字符开头并一直运行到行尾。
电脑无视评论，
但它们对于帮助人们（包括你未来的自己）理解和使用脚本非常宝贵。
唯一需要注意的是，每次修改脚本时，
您应该检查评论是否仍然准确：
将读者引向错误方向的解释比根本没有更糟糕。

如果我们想在单个管道中处理多个文件怎么办？
例如，如果我们想按长度对 `.pdb` 文件进行排序，我们可以输入：

~~~
$ wc -l *.pdb | sort -n
~~~
{: .language-bash}

因为 `wc -l` 列出了文件中的行数
（回想一下，`wc` 代表“字数”，添加 `-l` 选项表示“计数行数”）
并且 `sort -n` 以数字方式对事物进行排序。
我们可以把它放在一个文件中，
但它只会对当前目录中的“.pdb”文件列表进行排序。
如果我们希望能够获得其他类型文件的排序列表，
我们需要一种方法将所有这些名称放入脚本中。
我们不能使用 `$1`、`$2` 等等
因为我们不知道有多少文件。
相反，我们使用特殊变量`$@`，
意思是，
'shell 脚本的所有命令行参数'。
我们还应该将`$@`放在双引号内
处理包含空格的参数的情况
（`"$@"` 是特殊语法，相当于`"$1"` `"$2"` ...)。

这是一个例子：

~~~
$ nano sorted.sh
~~~
{: .language-bash}

~~~
# Sort files by their length.
# Usage: bash sorted.sh one_or_more_filenames
wc -l "$@" | sort -n
~~~
{: .source}

~~~
$ bash sorted.sh *.pdb ../creatures/*.dat
~~~
{: .language-bash}

~~~
9 methane.pdb
12 ethane.pdb
15 propane.pdb
20 cubane.pdb
21 pentane.pdb
30 octane.pdb
163 ../creatures/basilisk.dat
163 ../creatures/minotaur.dat
163 ../creatures/unicorn.dat
596 total
~~~
{: .output}

> ## 列出独特的物种
>
> Leah 有数百个数据文件，每个文件的格式如下：
>
> ~~~
> 2013-11-05,deer,5
> 2013-11-05,rabbit,22
> 2013-11-05,raccoon,7
> 2013-11-06,rabbit,19
> 2013-11-06,deer,2
> 2013-11-06,fox,1
> 2013-11-07,rabbit,18
> 2013-11-07,bear,1
> ~~~
> {: .source}
>
> `shell-lesson-data/data/animal-counts/animals.txt` 中给出了此类文件的示例。
>
> 我们可以使用命令`cut -d , -f 2 animals.txt | 排序 | uniq`来生产
> `animals.txt` 中的独特物种。
> 为了避免每次都要打出这一系列命令，
> 科学家可能会选择编写一个 shell 脚本。
>
> 编写一个名为 `species.sh` 的 shell 脚本，它接受任意数量的
> 文件名作为命令行参数，并使用上述命令的变体
> 分别打印出现在每个文件中的独特物种的列表。
>
> > ## 解决方案
> >
> > ```
> > # 脚本在 csv 文件中查找唯一物种，其中物种是第二个数据字段
> > # 这个脚本接受任意数量的文件名作为命令行参数
> >
> > # Loop over all files
> > for file in $@
> > do
> >     echo "Unique species in $file:"
> >     # Extract species names
> >     cut -d , -f 2 $file | sort | uniq
> > done
> > ```
> > {: .language-bash}
> {: .solution}
{: .challenge}


假设我们刚刚运行了一系列有用的命令——例如，
它创建了一个我们想在论文中使用的图表。
如果需要，我们希望以后能够重新创建图表，
所以我们想将命令保存在一个文件中。
而不是再次输入它们
（并可能让他们错了）
我们做得到：

~~~
$ history | tail -n 5 > redo-figure-3.sh
~~~
{: .language-bash}

文件 `redo-figure-3.sh` 现在包含：

~~~
297 bash goostats.sh NENE01729B.txt stats-NENE01729B.txt
298 bash goodiff.sh stats-NENE01729B.txt /data/validated/01729.txt > 01729-differences.txt
299 cut -d ',' -f 2-3 01729-differences.txt > 01729-time-series.txt
300 ygraph --format scatter --color bw --borders none 01729-time-series.txt figure-3.png
301 history | tail -n 5 > redo-figure-3.sh
~~~
{: .source}

在编辑器中删除命令上的序列号后，
并删除我们调用“历史”命令的最后一行，
我们完全准确地记录了我们如何创建该数字。

> ## 为什么在运行命令之前将命令记录在历史记录中？
>
> 如果您运行命令：
>
> ~~~
> $ history | tail -n 5 > recent.sh
> ~~~
> {: .language-bash}
>
> 文件中的最后一个命令是 `history` 命令本身，即，
> 实际上，shell 已经在命令日志中添加了 `history`
> 运行它。 实际上，shell *always* 将命令添加到日志中
> 在运行它们之前。 为什么你认为它会这样做？
>
> > ## 解决方案
> > 如果一个命令导致某些东西崩溃或挂起，它可能很有用
> > 了解该命令是什么，以便调查问题。
> > 如果命令只在运行后记录，我们就不会
> > 在发生崩溃时记录最后运行的命令。
> {: .solution}
{: .challenge}

在实践中，大多数人通过在 shell 提示符下运行几次命令来开发 shell 脚本
确保他们在做正确的事，
然后将它们保存在文件中以供重复使用。
这种工作方式让人们可以循环利用
他们通过一次调用“历史”来发现他们的数据和工作流程
并进行一些编辑以清理输出
并将其保存为 shell 脚本。

## Nelle 的管道：创建脚本


Nelle 的主管坚持认为她的所有分析都必须是可重复的。
捕获所有步骤的最简单方法是在脚本中。

首先我们回到 Nelle 的数据目录：
```
$ cd ../north-pacific-gyre/2012-07-03/
```
{: .language-bash}

她运行编辑器并编写以下内容：

~~~
# 计算数据文件的统计数据。
for datafile in "$@"
do
    echo $datafile
    bash goostats.sh $datafile stats-$datafile
done
~~~
{: .language-bash}

她将其保存在一个名为“do-stats.sh”的文件中
这样她现在可以通过键入以下内容重新进行第一阶段的分析：

~~~
$ bash do-stats.sh NENE*A.txt NENE*B.txt
~~~
{: .language-bash}

她也可以这样做：

~~~
$ bash do-stats.sh NENE*A.txt NENE*B.txt | wc -l
~~~
{: .language-bash}

所以输出只是处理的文件数
而不是已处理文件的名称。

关于 Nelle 的剧本需要注意的一件事是
它让运行它的人决定要处理哪些文件。
她可以这样写：
~~~
# 计算站点 A 和站点 B 数据文件的统计数据。
for datafile in NENE*A.txt NENE*B.txt
do
    echo $datafile
    bash goostats.sh $datafile stats-$datafile
done
~~~
{: .language-bash}

这样做的好处是总是选择正确的文件：
她不必记住排除“Z”文件。
缺点是它*总是*只选择那些文件——她不能在所有文件上运行它
（包括“Z”文件），
或者在她在南极洲的同事正在制作的“G”或“H”文件上，
无需编辑脚本。
如果她想更冒险，
她可以修改她的脚本来检查命令行参数，
如果没有提供，请使用`NENE*A.txt NENE*B.txt`。
当然，这在灵活性和复杂性之间引入了另一种权衡。

> ## Shell 脚本中的变量
>
> 在 `molecules` 目录中，假设您有一个名为 `script.sh` 的 shell 脚本，其中包含
> 以下命令：
>
> ~~~
> head -n $2 $1
> tail -n $3 $1
> ~~~
> {: .language-bash}
>
> 当您在 `molecules` 目录中时，键入以下命令：
>
> ~~~
> $ bash script.sh '*.pdb' 1 1
> ~~~
> {: .language-bash}
>
> 您希望看到以下哪些输出？
>
> 1. 以`.pdb`结尾的每个文件的第一行和最后一行之间的所有行
> 在“分子”目录中
> 2. `molecules`目录下每个以`.pdb`结尾的文件的第一行和最后一行
> 3. `molecules`目录下每个文件的第一行和最后一行
> 4. `*.pdb` 周围的引号引起的错误
>
> > ## 解决方案
> > 正确答案是2。
> >
> > 特殊变量 $1、$2 和 $3 表示赋予给
>> 脚本，这样运行的命令是：
> >
> > ```
> > $ head -n 1 cubane.pdb ethane.pdb octane.pdb pentane.pdb propane.pdb
> > $ tail -n 1 cubane.pdb ethane.pdb octane.pdb pentane.pdb propane.pdb
> > ```
> > {: .language-bash}
> > shell 不会扩展 `'*.pdb'`，因为它是用引号括起来的。
> > 因此，脚本的第一个参数是`'*.pdb'`，它在
> > 由 `head` 和 `tail` 编写的脚本。
> {: .solution}
{: .challenge}

> ## 查找具有给定扩展名的最长文件
>
> 编写一个名为 `longest.sh` 的 shell 脚本，其名称为
> 目录和文件扩展名作为其参数，并打印
> 取出该目录中行数最多的文件名
> 使用该扩展名。 例如：
>
> ~~~
> $ bash longest.sh shell-lesson-data/data/pdb pdb
> ~~~
> {: .language-bash}
>
> 将在 `shell-lesson-data/data/pdb` 中打印 `.pdb` 文件的名称
> 最多的线路。
>
> 随意在另一个目录上测试您的脚本，例如
> ~~~
> $ bash longest.sh shell-lesson-data/writing/data txt
> ~~~
> {: .language-bash}
>
> > ## Solution
> >
> > ```
> > # Shell 脚本有两个参数：
> > # 1.一个目录名
> > # 2. 文件扩展名
> > # 并打印该目录中的文件名
> > # 与文件扩展名匹配的行数最多。
> >
> > wc -l $1/*.$2 | sort -n | tail -n 2 | head -n 1
> > ```
> > {: .language-bash}
> >
> > 管道的第一部分，`wc -l $1/*.$2 | 排序-n`，计数
> > 每个文件中的行并按数字排序（最大的最后）。 什么时候
> > 文件不止一个，`wc` 还输出最后的摘要行，
> > 给出 _all_ 文件的总行数。 我们使用`tail
> > -n 2 | head -n 1` 丢弃最后一行。
> >
> > 使用`wc -l $1/*.$2 | 排序-n | tail -n 1` 我们会看到最后的总结
> > line：我们可以分段构建管道以确保我们理解
> > 输出。
> >
> {: .solution}
{: .challenge}

> ## 脚本阅读理解
>
> 对于这个问题，请再次考虑 `shell-lesson-data/molecules` 目录。
> 这包含许多 `.pdb` 文件，以及您使用的任何其他文件
> 可能已经创建。
> 解释以下三个脚本中的每一个在运行时会做什么
> 分别为 `bash script1.sh *.pdb`、`bash script2.sh *.pdb` 和 `bash script3.sh *.pdb`。
>
> ~~~
> # Script 1
> echo *.*
> ~~~
> {: .language-bash}
>
> ~~~
> # Script 2
> for filename in $1 $2 $3
> do
>     cat $filename
> done
> ~~~
> {: .language-bash}
>
> ~~~
> # Script 3
> echo $@.pdb
> ~~~
> {: .language-bash}
>
> > ## 解决方案
>> 在每种情况下，shell 在传递结果之前扩展 `*.pdb` 中的通配符
>> 作为脚本参数的文件名列表。
> >
> > 脚本 1 将打印出名称中包含点的所有文件的列表。
> > 传递给脚本的参数实际上并未在脚本中的任何地方使用。
> >
> > 脚本 2 将打印前 3 个文件的内容，文件扩展名为 `.pdb`。
>> `$1`、`$2` 和 `$3` 分别指第一个、第二个和第三个参数。
> >
> > 脚本 3 将打印脚本的所有参数（即所有 `.pdb` 文件），
> > 后跟`.pdb`。
> > `$@` 指的是 *所有* 给 shell 脚本的参数。
> > ```
> > cubane.pdb ethane.pdb methane.pdb octane.pdb pentane.pdb propane.pdb.pdb
> > ```
> > {: .output}
> {: .solution}
{: .challenge}

> ## 调试脚本
>
> 假设您已将以下脚本保存在名为 `do-errors.sh` 的文件中
> 在 Nelle 的 `north-pacific-gyre/2012-07-03` 目录中：
>
> ~~~
> # 计算数据文件的统计数据。
> for datafile in "$@"
> do
>     echo $datfile
>     bash goostats.sh $datafile stats-$datafile
> done
> ~~~
> {: .language-bash}
>
> 当你运行它时：
>
> ~~~
> $ bash do-errors.sh NENE*A.txt NENE*B.txt
> ~~~
> {: .language-bash}
>
> 输出为空白。
> 要找出原因，请使用 `-x` 选项重新运行脚本：
>
> ~~~
> $ bash -x do-errors.sh NENE*A.txt NENE*B.txt
> ~~~
> {: .language-bash}
>
> 输出显示给你什么？
> 哪条线路对错误负责？
>
> > ## 解决方案
> > `-x` 选项使 `bash` 在调试模式下运行。
> > 这会在运行时打印出每个命令，这将帮助您定位错误。
> > 在这个例子中，我们可以看到 `echo` 没有打印任何东西。 我们打错了
>> 在循环变量名中，变量`datfile`不存在，因此返回
> > 一个空字符串。
> {: .solution}
{: .challenge}

{% include links.md %}
