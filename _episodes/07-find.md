---
title: "查找"
teaching: 25
exercises: 20
questions:
- "我怎样才能找到文件？"
- "如何在文件中查找内容？"
objectives:
- "使用 `grep` 从文本文件中选择与简单模式匹配的行。"
- "使用 `find` 查找名称与简单模式匹配的文件和目录。"
- "使用一个命令的输出作为另一个命令的命令行参数。"
- "解释“文本”和“二进制”文件的含义，以及为什么许多常用工具不能很好地处理后者。"
keypoints:
- "`find` 查找具有匹配模式的特定属性的文件。"
- "`grep` 选择文件中匹配模式的行。"
- "`--help` 是许多 bash 命令支持的选项，以及可以在 Bash 中运行的程序，以显示有关如何使用这些命令或程序的更多信息。"
- "`man [command]` 显示给定命令的手册页。"
- "`$([command])` 将命令的输出插入到位。"
---

就像我们许多人现在使用“Google”作为
动词意思是“找到”，Unix 程序员经常使用
单词'grep'。
'grep' 是 'global/regular expression/print' 的缩写，
早期 Unix 文本编辑器中的常见操作序列。
它也是一个非常有用的命令行程序的名称。

`grep` 查找并打印文件中与模式匹配的行。
对于我们的示例，
我们将使用一个包含三个俳句的文件，取自
1998 年在 *Salon* 杂志上的比赛。 对于这组示例，
我们将在写作子目录中工作：

~~~
$ cd
$ cd Desktop/shell-lesson-data/writing
$ cat haiku.txt
~~~
{: .language-bash}

~~~
The Tao that is seen
Is not the true Tao, until
You bring fresh toner.

With searching comes loss
and the presence of absence:
"My Thesis" not found.

Yesterday it worked
Today it is not working
Software is like that.
~~~
{: .output}

> ## 永远，或五年
>
> 我们没有链接到原始的俳句，因为
> 他们似乎不再出现在 *Salon* 的网站上。
> 正如 [Jeff Rothenberg 所说](https://www.clir.org/wp-content/uploads/sites/6/ensuring.pdf)，
> “数字信息永远存在——或五年，以先到者为准。”
> 幸运的是，热门内容通常[有备份](http://wiki.c2.com/?ComputerErrorHaiku)。
{: .callout}

让我们找到包含单词“not”的行：

~~~
$ grep not haiku.txt
~~~
{: .language-bash}

~~~
Is not the true Tao, until
"My Thesis" not found
Today it is not working
~~~
{: .output}

在这里，`not` 是我们要搜索的模式。
grep 命令搜索文件，查找与指定模式匹配的内容。
要使用它，输入 `grep`，然后是我们正在搜索的模式，最后
我们正在搜索的文件（或多个文件）的名称。

输出是文件中包含字母“not”的三行。

默认情况下，grep 以区分大小写的方式搜索模式。
另外，我们选择的搜索模式不一定要组成一个完整的词，
正如我们将在下一个示例中看到的那样。

让我们搜索模式：'The'。

~~~
$ grep The haiku.txt
~~~
{: .language-bash}

~~~
The Tao that is seen
"My Thesis" not found.
~~~
{: .output}

这一次，输出包含字母“The”的两行，
其中一个包含我们在一个更大的词“论文”中的搜索模式。

要将匹配限制为单独包含单词“The”的行，
我们可以给 `grep` 加上 `-w` 选项。
这会将匹配限制在单词边界内。

在本课的后面，我们还将看到如何更改 grep 的搜索行为
关于它的大小写敏感性。

~~~
$ grep -w The haiku.txt
~~~
{: .language-bash}

~~~
The Tao that is seen
~~~
{: .output}

请注意，“单词边界”包括行的开头和结尾，因此不包括
只是被空格包围的字母。
有时我们不
想搜索一个词，但是一个词组。 这也很容易做到
`grep` 通过将短语放在引号中。

~~~
$ grep -w "is not" haiku.txt
~~~
{: .language-bash}

~~~
Today it is not working
~~~
{: .output}

我们现在已经看到，您不必在单个单词周围加上引号，
但是在搜索多个单词时使用引号很有用。
它还有助于更容易区分搜索词或短语
和正在搜索的文件。
我们将在其余示例中使用引号。

另一个有用的选项是 `-n`，它为匹配的行编号：

~~~
$ grep -n "it" haiku.txt
~~~
{: .language-bash}

~~~
5:With searching comes loss
9:Yesterday it worked
10:Today it is not working
~~~
{: .output}

在这里，我们可以看到第 5、9 和 10 行包含字母“it”。

我们可以像使用其他 Unix 命令一样组合选项（即标志）。
例如，让我们找到包含单词“the”的行。
我们可以结合选项 `-w` 来查找包含单词 'the' 的行
和 `-n` 对匹配的行进行编号：

~~~
$ grep -n -w "the" haiku.txt
~~~
{: .language-bash}

~~~
2:Is not the true Tao, until
6:and the presence of absence:
~~~
{: .output}

现在我们想使用选项 `-i` 使我们的搜索不区分大小写：

~~~
$ grep -n -w -i "the" haiku.txt
~~~
{: .language-bash}

~~~
1:The Tao that is seen
2:Is not the true Tao, until
6:and the presence of absence:
~~~
{: .output}

现在，我们想使用选项 `-v` 来反转我们的搜索，即我们想要输出
不包含单词“the”的行。

~~~
$ grep -n -w -v "the" haiku.txt
~~~
{: .language-bash}

~~~
1:The Tao that is seen
3:You bring fresh toner.
4:
5:With searching comes loss
7:"My Thesis" not found.
8:
9:Yesterday it worked
10:Today it is not working
11:Software is like that.
~~~
{: .output}


如果我们使用 `-r`（递归）选项，
`grep` 可以通过子目录中的一组文件递归地搜索模式。

让我们在 `shell-lesson-data/writing` 目录中递归搜索 `Yesterday`：

```
$ grep -r Yesterday .
```
{: .language-bash}

```
data/LittleWomen.txt:"Yesterday, when Aunt was asleep and I was trying to be as still as a
data/LittleWomen.txt:Yesterday at dinner, when an Austrian officer stared at us and then
data/LittleWomen.txt:Yesterday was a quiet day spent in teaching, sewing, and writing in my
haiku.txt:Yesterday it worked
```
{: .output}

`grep` 有很多其他选项。 要找出它们是什么，我们可以输入：

~~~
$ grep --help
~~~
{: .language-bash}

~~~
Usage: grep [OPTION]... PATTERN [FILE]...
Search for PATTERN in each FILE or standard input.
PATTERN is, by default, a basic regular expression (BRE).
Example: grep -i 'hello world' menu.h main.c

Regexp selection and interpretation:
  -E, --extended-regexp     PATTERN is an extended regular expression (ERE)
  -F, --fixed-strings       PATTERN is a set of newline-separated fixed strings
  -G, --basic-regexp        PATTERN is a basic regular expression (BRE)
  -P, --perl-regexp         PATTERN is a Perl regular expression
  -e, --regexp=PATTERN      use PATTERN for matching
  -f, --file=FILE           obtain PATTERN from FILE
  -i, --ignore-case         ignore case distinctions
  -w, --word-regexp         force PATTERN to match only whole words
  -x, --line-regexp         force PATTERN to match only whole lines
  -z, --null-data           a data line ends in 0 byte, not newline

Miscellaneous:
...        ...        ...
~~~
{: .output}

> ## 使用`grep`
>
> 哪个命令会产生以下输出：
>
> ~~~
> and the presence of absence:
> ~~~
> {: .output}
>
> 1. `grep "of" haiku.txt`
> 2. `grep -E "of" haiku.txt`
> 3. `grep -w "of" haiku.txt`
> 4. `grep -i "of" haiku.txt`
>
> > ## 解决方案
> > 正确答案是 3，因为 `-w` 选项只查找全字匹配。
> > 当是另一个单词的一部分时，其他选项也将匹配“of”。
> {: .solution}
{: .challenge}

> ## 通配符
>
> 不过，`grep` 的真正威力并不来自于它的选项； 它来自
> 模式可以包含通配符的事实。 （技术名称为
> 这些是**正则表达式**，其中
> 是 'grep' 中的 're' 所代表的意思。）正则表达式都很复杂
> 并且功能强大； 如果你想做复杂的搜索，请看课程
> 在 [我们的网站](http://v4.software-carpentry.org/regexp/index.html)。 作为品酒师，我们可以
> 查找第二个位置有“o”的行，如下所示：
>
> ~~~
> $ grep -E "^.o" haiku.txt
> ~~~
> {: .language-bash}
>
> ~~~
> You bring fresh toner.
> Today it is not working
> Software is like that.
> ~~~
> {: .output}
>
> 我们使用 `-E` 选项并将模式放在引号中以防止 shell
> 从试图解释它。 （如果模式包含一个 `*`，对于
> 例如，shell 会在运行 `grep` 之前尝试扩展它。）
> 模式中的 `^` 将匹配锚定到行首。 `.`
> 匹配单个字符（就像 shell 中的 `?`），而 `o`
> 匹配实际的“o”。
{: .callout}

> ## 追踪物种
>
> Leah 有几百个
> 数据文件保存在一个目录中，每个目录的格式如下：
>
> ~~~
> 2013-11-05,deer,5
> 2013-11-05,rabbit,22
> 2013-11-05,raccoon,7
> 2013-11-06,rabbit,19
> 2013-11-06,deer,2
> ~~~
> {: .source}
>
> 她想编写一个 shell 脚本，将物种作为第一个命令行参数
> 和一个目录作为第二个参数。 该脚本应返回一个名为“species.txt”的文件
> 包含日期列表和在每个日期看到的该物种的数量。
> 例如，使用上面显示的数据，`rabbit.txt` 将包含：
>
> ~~~
> 2013-11-05,22
> 2013-11-06,19
> ~~~
> {: .source}
>
> 以正确的顺序放置这些命令和管道以实现此目的：
>
> ~~~
> cut -d : -f 2
> >
> |
> grep -w $1 -r $2
> |
> $1.txt
> cut -d , -f 1,3
> ~~~
> {: .language-bash}
>
> 提示：使用 `man grep` 查找如何在目录中递归地 grep 文本
> 和 `man cut` 在一行中选择多个字段。
>
> `shell-lesson-data/data/animal-counts/animals.txt` 中提供了此类文件的示例
>
> > ## 解决方案
> >
> > ```
> > grep -w $1 -r $2 | cut -d : -f 2 | cut -d , -f 1,3 > $1.txt
> > ```
> > {: .source}
> >
> > 其实你可以把两个cut命令的顺序调换一下，还是可以的。 在
> > 命令行，尝试更改剪切命令的顺序，并查看输出
> > 从每一步看为什么会这样。
> >
> > 你可以这样调用上面的脚本：
> >
> > ```
> > $ bash count-species.sh bear .
> > ```
> > {: .language-bash}
> {: .solution}
{: .challenge}

> ## 小女人
>
> 你和你的朋友，刚刚读完 *Little Women* by
> Louisa May Alcott，正在争论中。 在四姐妹中
> book、Jo、Meg、Beth 和 Amy，你的朋友认为 Jo 是
> 提到最多的。 但是，您可以确定是艾米。 幸运的是，你
> 有一个包含小说全文的文件“LittleWomen.txt”
> (`shell-lesson-data/writing/data/LittleWomen.txt`)。
> 使用 `for` 循环，如何将每个循环的次数制成表格
> 提到四姐妹？
>
> 提示：一种解决方案可能会采用
> 命令 `grep` 和 `wc` 和一个 `|`，而另一个可能会使用
> `grep` 选项。
> 解决编程任务的方法通常不止一种，因此
> 特定的解决方案通常是根据以下组合选择的
> 产生正确的结果、优雅、可读性和速度。
>
> > ## 解决方案
> > ```
> > for sis in Jo Meg Beth Amy
> > do
> >     echo $sis:
> >     grep -ow $sis LittleWomen.txt | wc -l
> > done
> > ```
> > {: .source}
> >
> > 替代的，略逊一筹的解决方案：
> > ```
> > for sis in Jo Meg Beth Amy
> > do
> >     echo $sis:
> >     grep -ocw $sis LittleWomen.txt
> > done
> > ```
> > {: .source}
> >
> > 这个解决方案比较差，因为 `grep -c` 只报告匹配的行数。
> > 如果有更多匹配项，则此方法报告的总匹配数会更低
> > 每行一个匹配。
> >
> > 敏锐的观察者可能已经注意到字符名称有时以全大写形式出现
> > 在章节标题中（例如“MEG GOES TO VANITY FAIR”）。
> > 如果你也想计算这些，你可以添加 `-i` 选项以不区分大小写
> >（虽然在这种情况下，它不影响提到哪个姐姐的答案
> > 最常见）。
> {: .solution}
{: .challenge}

当 `grep` 在文件中查找行时，
`find` 命令自己查找文件。
再次，
它有很多选择；
为了展示最简单的如何工作，我们将使用下面显示的目录树。

![“writing”目录下的文件树包含几个子目录和
文件，例如“writing”包含目录“data”、“thesis”、“tools”和
文件“haiku.txt”； “writing/data”包含文件“Little Women.txt”，
“one.txt”和“two.txt”； “写作/论文”包含文件“empty-draft.md”；
“writing/tools”包含目录“old”和文件“format”和“stats”；
并且“writing/tools/old”包含一个文件“oldtool”](../fig/find-file-tree.svg)

Nelle 的 `writing` 目录包含一个名为 `haiku.txt` 的文件和三个子目录：
`thesis`（其中包含一个可悲的空文件，`empty-draft.md`）；
`data`（包含三个文件`LittleWomen.txt`、`one.txt`和`two.txt`）；
以及一个包含程序“format”和“stats”的“tools”目录，
和一个名为“old”的子目录，其中包含一个文件“oldtool”。

对于我们的第一个命令，
让我们运行 `find .`（记得从 `shell-lesson-data/writing` 文件夹运行此命令）。

~~~
$ find .
~~~
{: .language-bash}

~~~
.
./data
./data/one.txt
./data/LittleWomen.txt
./data/two.txt
./tools
./tools/format
./tools/old
./tools/old/oldtool
./tools/stats
./haiku.txt
./thesis
./thesis/empty-draft.md
~~~
{: .output}

一如既往，
`.` 本身表示当前工作目录，
这是我们希望搜索开始的地方。
`find` 的输出是每个文件的名称**和**目录
在当前工作目录下。
起初这似乎没用，但 `find` 有很多选项
过滤输出，在本课中我们将发现一些
其中。

我们列表中的第一个选项是
`-type d` 表示“目录的东西”。
果然，
`find` 的输出是我们的小树中五个目录的名称
（包括`.`）：

~~~
$ find . -type d
~~~
{: .language-bash}

~~~
./
./data
./thesis
./tools
./tools/old
~~~
{: .output}

请注意，“find”找到的对象没有按任何特定顺序列出。
如果我们将 `-type d` 更改为 `-type f`，
我们得到了所有文件的列表：

~~~
$ find . -type f
~~~
{: .language-bash}

~~~
./haiku.txt
./tools/stats
./tools/old/oldtool
./tools/format
./thesis/empty-draft.md
./data/one.txt
./data/LittleWomen.txt
./data/two.txt
~~~
{: .output}

现在让我们尝试按名称匹配：

~~~
$ find . -name *.txt
~~~
{: .language-bash}

~~~
./haiku.txt
~~~
{: .output}

我们希望它能找到所有的文本文件，
但它只打印出`./haiku.txt`。
问题是 shell 扩展了通配符，例如 `*` *before* 命令运行。
由于当前目录中的 `*.txt` 扩展为 `haiku.txt`，
我们实际运行的命令是：

~~~
$ find . -name haiku.txt
~~~
{: .language-bash}

`find` 完成了我们的要求； 我们只是要求错误的东西。

为了得到我们想要的，
让我们做我们对 `grep` 所做的事情：
将 `*.txt` 放在引号中以防止 shell 扩展 `*` 通配符。
这边走，
`find` 实际上得到的是 `*.txt` 模式，而不是扩展的文件名 `haiku.txt`：

~~~
$ find . -name "*.txt"
~~~
{: .language-bash}

~~~
./data/one.txt
./data/LittleWomen.txt
./data/two.txt
./haiku.txt
~~~
{: .output}

> ## 列出与查找
>
> `ls` 和 `find` 可以做类似的事情给定正确的选项，
> 但在正常情况下，
> `ls` 列出了它可以列出的所有内容，
> 而 `find` 搜索具有某些属性的事物并显示它们。
{: .callout}

正如我们之前所说，
命令行的力量在于组合工具。
我们已经看到了如何用管道做到这一点；
让我们看看另一种技术。
正如我们刚刚看到的，
`寻找。 -name "*.txt"` 为我们提供了当前目录中或之下的所有文本文件的列表。
我们如何将它与 `wc -l` 结合起来计算所有这些文件中的行数？

最简单的方法是将 `find` 命令放在 `$()` 中：

~~~
$ wc -l $(find . -name "*.txt")
~~~
{: .language-bash}

~~~
11 ./haiku.txt
300 ./data/two.txt
21022 ./data/LittleWomen.txt
70 ./data/one.txt
21403 total
~~~
{: .output}

当 shell 执行这个命令时，
它做的第一件事就是运行`$()`里面的任何东西。
然后它用该命令的输出替换 `$()` 表达式。
由于`find`的输出是四个文件名`./data/one.txt`, `./data/LittleWomen.txt`,
`./data/two.txt` 和 `./haiku.txt`，shell 构造命令：

~~~
$ wc -l ./data/one.txt ./data/LittleWomen.txt ./data/two.txt ./haiku.txt
~~~
{: .language-bash}

这就是我们想要的。
这种扩展正是 shell 在扩展像 `*` 和 `?` 这样的通配符时所做的，
但是让我们使用我们想要的任何命令作为我们自己的“通配符”。

`find` 和 `grep` 一起使用是很常见的。
第一个查找匹配模式的文件；
第二个在那些文件中查找与另一个模式匹配的行。
例如，在这里，我们可以找到包含铁原子的 PDB 文件
通过在当前目录上方的所有“.pdb”文件中查找字符串“FE”：

~~~
$ grep "FE" $(find .. -name "*.pdb")
~~~
{: .language-bash}

~~~
../data/pdb/heme.pdb:ATOM     25 FE           1      -0.924   0.535  -0.518
~~~
{: .output}

> ## 匹配和减法
>
> `grep` 的 `-v` 选项反转模式匹配，因此只有行
> 不匹配模式的将被打印。 鉴于此，哪一个
> 以下命令将在 `/data` 中查找所有文件名
> 以 `s.txt` 结尾，但谁的名字*不*包含字符串 `net`？
> （例如，`animals.txt` 或 `amino-acids.txt` 但不是 `planets.txt`。）
> 一旦你想好了你的答案，你就可以测试 `shell-lesson-data` 中的命令了
> 目录。
>
> 1.`查找数据-名称“*s.txt” | grep -v 网络`
> 2.`查找数据-name *s.txt | grep -v 网络`
> 3. `grep -v "net" $(find data -name "*s.txt")`
> 4. 以上都不是。
>
> > ## 解决方案
> > 正确答案是 1. 将匹配表达式放在引号中可以防止shell
> > 扩展它，所以它被传递给`find` 命令。
> >
> > 选项 2 不正确，因为 shell 扩展了 `*s.txt` 而不是传递通配符
> > 表达式到`find`。
> >
> > 选项 3 不正确，因为它在文件内容中搜索
> > 不匹配'net'，而不是搜索文件名。
> {: .solution}
{: .challenge}

> ## 二进制文件
>
> 我们专注于在文本文件中寻找模式。如果
> 您的数据是以图像、数据库或其他格式存储的？
>
> 一些工具扩展了 `grep` 来处理一些非文本格式。但是一个
> 更通用的方法是将数据转换为文本，或者
> 从数据中提取类似文本的元素。一方面，它变得简单
> 事情容易做。另一方面，复杂的事情通常是不可能的。为了
> 例如，编写一个提取 X 和 Y 的程序很容易
> 来自图像文件的尺寸供 `grep` 使用，但你会怎么做
> 写一些东西来查找电子表格中的值，其单元格包含
> 公式？
>
> 最后一个选择是认识到 shell 和文本处理具有
> 他们的限制，并使用另一种编程语言。
> 到了做这件事的时候，不要对外壳太苛刻：很多
> 现代编程语言借鉴了很多
> 创意来源于它，模仿也是最真诚的赞美。
{: .callout}

Linux shell比大多数使用它的人都要老。 它有
幸存了这么久，因为它是最高效的编程之一
曾经创造的环境——甚至可能是*最有生产力的。 它的语法
可能很神秘，但掌握它的人可以尝试
以交互方式使用不同的命令，然后使用他们所学的
自动化他们的工作。 图形用户界面可能更易于使用
首先，但一旦学会，shell中的生产力是无与伦比的。

> ## `find` 流水线阅读理解
>
> 为以下 shell 脚本写一个简短的解释性注释：
>
> ~~~
> wc -l $(find . -name "*.dat") | sort -n
> ~~~
> {: .language-bash}
>
> > ## 解决方案
> > 1. 从当前目录递归查找所有带有`.dat`扩展名的文件
> > 2. 计算每个文件包含的行数
> > 3. 对步骤 2 的输出进行数字排序
> {: .solution}
{: .challenge}


{% include links.md %}
