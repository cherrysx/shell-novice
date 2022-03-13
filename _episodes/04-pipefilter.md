---
title: "管道和过滤器"
teaching: 25
exercises: 10
questions:
- "我如何结合现有的命令来做新的事情？"
objectives:
- "将命令的输出重定向到文件。"
- "构建具有两个或更多阶段的命令管道。"
- "解释如果程序或管道没有任何输入来处理通常会发生什么。"
- "解释将命令与管道和过滤器链接的优势。"
keypoints:
- "`wc` 计算输入中的行数、单词和字符。"
- "`cat` 显示输入的内容。"
- "`sort` 对其输入进行排序。"
- "`head` 显示其输入的前 10 行。"
- "`tail` 显示其输入的最后 10 行。"
- "`command > [file]` 将命令的输出重定向到文件（覆盖任何现有内容）。"
- "`command >> [file]` 将命令的输出附加到文件中。"
- "`[第一个] | [second]` 是一个管道：第一个命令的输出用作第二个命令的输入。"
- "使用 shell 的最佳方式是使用管道来组合简单的单一用途程序（过滤器）。"
---

现在我们知道了一些基本的命令，
我们终于可以看看 shell 最强大的功能了：
它让我们可以轻松地以新的方式组合现有程序。
我们将从名为“shell-lesson-data/molecules”的目录开始
其中包含六个文件，描述了一些简单的有机分子。
`.pdb` 扩展名表示这些文件是蛋白质数据库格式，
一种简单的文本格式，用于指定分子中每个原子的类型和位置。

~~~
$ ls molecules
~~~
{: .language-bash}

~~~
cubane.pdb    methane.pdb    pentane.pdb
ethane.pdb    octane.pdb     propane.pdb
~~~
{: .output}

让我们使用 `cd` 进入该目录并运行示例命令 `wc cubane.pdb`：

~~~
$ cd molecules
$ wc cubane.pdb
~~~
{: .language-bash}

~~~
20  156 1158 cubane.pdb
~~~
{: .output}

`wc` 是“字数统计”命令：
它计算文件中的行数、单词数和字符数（从左到右，按此顺序）。

如果我们运行命令 `wc *.pdb`，`*.pdb` 中的 `*` 匹配零个或多个字符，
因此 shell 将 `*.pdb` 转换为当前目录中所有 `.pdb` 文件的列表：

~~~
$ wc *.pdb
~~~
{: .language-bash}

~~~
  20  156  1158  cubane.pdb
  12  84   622   ethane.pdb
   9  57   422   methane.pdb
  30  246  1828  octane.pdb
  21  165  1226  pentane.pdb
  15  111  825   propane.pdb
 107  819  6081  total
~~~
{: .output}

请注意，`wc *.pdb` 还会在输出的最后一行显示所有行的总数。

如果我们运行 `wc -l` 而不是 `wc`，
输出仅显示每个文件的行数：

~~~
$ wc -l *.pdb
~~~
{: .language-bash}

~~~
  20  cubane.pdb
  12  ethane.pdb
   9  methane.pdb
  30  octane.pdb
  21  pentane.pdb
  15  propane.pdb
 107  total
~~~
{: .output}

`-m` 和 `-w` 选项也可以与 `wc` 命令一起使用，以显示
只有文件中的字符数或单词数。

> ## 为什么它什么都不做？
>
> 如果一个命令应该处理一个文件会发生什么，但是我们
> 不要给它一个文件名？ 例如，如果我们键入：
>
> ~~~
> $ wc -l
> ~~~
> {: .language-bash}
>
> 但不要在命令后键入`*.pdb`（或其他任何内容）？
> 因为它没有任何文件名，`wc` 假定它应该
> 处理在命令提示符下给出的输入，所以它只是坐在那里等待我们给出
> 它以交互方式获取一些数据。 然而，从外面看，我们所看到的只是它
> 坐在那里：命令似乎没有做任何事情。
>
> 如果你犯了这种错误，你可以通过按住退出这个状态
> 控制键 (<kbd>Ctrl</kbd>) 并键入字母 <kbd>C</kbd> 一次，然后
> 松开 <kbd>Ctrl</kbd> 键。
> <kbd>Ctrl</kbd>+<kbd>C</kbd>
{: .callout}


## 捕获命令的输出

这些文件中哪个包含最少的行？
当只有六个文件时，这是一个容易回答的问题，
但是如果有 6000 个呢？
我们迈向解决方案的第一步是运行以下命令：

~~~
$ wc -l *.pdb > lengths.txt
~~~
{: .language-bash}

大于号 `>` 告诉 shell **redirect** 命令的输出
到文件而不是打印到屏幕上。 （这就是没有屏幕输出的原因：
`wc` 会打印的所有内容都已进入
文件 `lengths.txt` 代替。）shell 将创建
如果文件不存在。 如果文件存在，它将是
静默覆盖，这可能会导致数据丢失，因此需要
一些谨慎。
`ls lengths.txt` 确认文件存在：

~~~
$ ls lengths.txt
~~~
{: .language-bash}

~~~
lengths.txt
~~~
{: .output}

我们现在可以使用 `cat lengths.txt` 将 `lengths.txt` 的内容发送到屏幕。
`cat` 命令的名称来自 'concatenate'，即连接在一起，
它一个接一个地打印文件的内容。
在这种情况下只有一个文件，
所以 `cat` 只是向我们展示了它包含的内容：

~~~
$ cat lengths.txt
~~~
{: .language-bash}

~~~
  20  cubane.pdb
  12  ethane.pdb
   9  methane.pdb
  30  octane.pdb
  21  pentane.pdb
  15  propane.pdb
 107  total
~~~
{: .output}

> ## 逐页输出
>
> 为了方便和一致，我们将在本课中继续使用 `cat`，
> 但它的缺点是它总是将整个文件转储到您的屏幕上。
> 在实践中更有用的是命令 `less`，
> 与 `less lengths.txt` 一起使用。
> 这会显示一屏文件，然后停止。
> 按空格键可以前进一屏，
> 或按“b”返回一个。 按“q”退出。
{: .callout}


## 过滤输出

接下来我们将使用 `sort` 命令对 `lengths.txt` 文件的内容进行排序。
但首先我们将使用一个练习来了解一些关于 sort 命令的知识：

> ## `sort -n` 有什么作用？
>
> 文件 [`shell-lesson-data/numbers.txt`](../shell-lesson-data/numbers.txt)
> 包含以下几行：
>
> ~~~
> 10
> 2
> 19
> 22
> 6
> ~~~
> {: .source}
>
> 如果我们对这个文件运行`sort`，输出是：
>
> ~~~
> 10
> 19
> 2
> 22
> 6
> ~~~
> {: .output}
>
> 如果我们在同一个文件上运行 `sort -n`，我们会得到这个：
>
> ~~~
> 2
> 6
> 10
> 19
> 22
> ~~~
> {: .output}
>
> 解释为什么 `-n` 有这个效果。
>
> > ## 解决方案
> > `-n` 选项指定数字而非字母数字排序。
> {: .solution}
{: .challenge}

我们还将使用 `-n` 选项来指定排序是
数字而不是字母数字。
这*不会*更改文件；
相反，它将排序结果发送到屏幕：

~~~
$ sort -n lengths.txt
~~~
{: .language-bash}

~~~
  9  methane.pdb
 12  ethane.pdb
 15  propane.pdb
 20  cubane.pdb
 21  pentane.pdb
 30  octane.pdb
107  total
~~~
{: .output}


我们可以将排序后的行列表放在另一个名为“sorted-lengths.txt”的临时文件中
通过在命令后放置`> sorted-lengths.txt`，
就像我们使用 `>lengths.txt` 将 `wc` 的输出放入 `lengths.txt` 中一样。
一旦我们这样做了，
我们可以运行另一个名为 `head` 的命令来获取 `sorted-lengths.txt` 中的前几行：

~~~
$ sort -n lengths.txt > sorted-lengths.txt
$ head -n 1 sorted-lengths.txt
~~~
{: .language-bash}

~~~
  9  methane.pdb
~~~
{: .output}

使用带有 `head` 的 `-n 1` 告诉它
我们只想要文件的第一行；
`-n 20` 会得到前 20 个，
等等。
由于 `sorted-lengths.txt` 包含我们文件的长度，从最小到最大排序，
`head` 的输出必须是行数最少的文件。

> ## 重定向到同一个文件
>
> 尝试重定向是一个非常糟糕的主意
> 对文件进行操作的命令的输出
> 到同一个文件。 例如：
>
> ~~~
> $ sort -n lengths.txt > lengths.txt
> ~~~
> {: .language-bash}
>
> 做这样的事情可能会给你
> 不正确的结果和/或删除
> `lengths.txt` 的内容。
{: .callout}

> ## `>>` 是什么意思？
>
> 我们已经看到了`>`的使用，但是有一个类似的操作符`>>`
> 其工作方式略有不同。
> 我们将通过打印一些字符串来了解这两个运算符之间的区别。
> 我们可以使用 `echo` 命令打印字符串，例如
>
> ~~~
> $ echo The echo command prints text
> ~~~
> {: .language-bash}
> ~~~
> The echo command prints text
> ~~~
> {: .output}
>
> 现在测试下面的命令以揭示两个运算符之间的区别：
>
> ~~~
> $ echo hello > testfile01.txt
> ~~~
> {: .language-bash}
>
> and:
>
> ~~~
> $ echo hello >> testfile02.txt
> ~~~
> {: .language-bash}
>
> 提示：尝试连续两次执行每个命令，然后检查输出文件。
>
> > ## 解决方案
> > 在第一个带有`>`的例子中，字符串'hello'被写入`testfile01.txt`，
> > 但是每次我们运行命令时文件都会被覆盖。
> >
>> 我们从第二个例子中看到`>>` 操作符也将 'hello' 写入文件
> > (在本例中为`testfile02.txt`),
> > 但如果字符串已经存在，则将其附加到文件中
> >（即当我们第二次运行它时）。
> {: .solution}
{: .challenge}

> ## 附加数据
>
> 我们已经遇到过 `head` 命令，它从文件的开头打印行。
> `tail` 类似，但从文件末尾打印行。
>
> 考虑文件 `shell-lesson-data/data/animals.txt`。
> 在这些命令之后，选择答案
> 对应于文件 `animals-subset.txt`：
>
> ~~~
> $ head -n 3 animals.txt > animals-subset.txt
> $ tail -n 2 animals.txt >> animals-subset.txt
> ~~~
> {: .language-bash}
>
> 1. `animals.txt`的前三行
> 2. `animals.txt`的最后两行
> 3. `animals.txt`的前三行和后两行
> 4. `animals.txt`的第二行和第三行
>
> > ## 解决方案
> > 选项 3 是正确的。
> > 为了使选项 1 正确，我们只运行 `head` 命令。
>> 为了使选项 2 正确，我们只运行 `tail` 命令。
>> 为了使选项 4 正确，我们必须将 `head` 的输出通过管道传输到 `tail -n 2`
>> 通过执行 `head -n 3 animals.txt | 尾巴 -n 2 > 动物-subset.txt`
> {: .solution}
{: .challenge}


## 将输出传递给另一个命令
在我们查找行数最少的文件的示例中，
我们使用两个中间文件“lengths.txt”和“sorted-lengths.txt”来存储输出。
这是一种令人困惑的工作方式，因为
即使你理解了 `wc`、`sort` 和 `head` 的作用，
那些中间文件让人很难跟踪正在发生的事情。
我们可以通过同时运行 `sort` 和 `head` 使其更容易理解：

~~~
$ sort -n lengths.txt | head -n 1
~~~
{: .language-bash}

~~~
  9  methane.pdb
~~~
{: .output}

两个命令之间的竖线 `|` 称为 **pipe**。
它告诉我们要使用的 shell
左侧命令的输出
作为右侧命令的输入。

这消除了对 `sorted-lengths.txt` 文件的需要。

## 组合多个命令
没有什么能阻止我们连续链接管道。
例如，我们可以将 `wc` 的输出直接发送到 `sort`，
然后将结果输出到`head`。
这消除了对任何中间文件的需要。

我们将首先使用管道将 `wc` 的输出发送到 `sort`：

~~~
$ wc -l *.pdb | sort -n
~~~
{: .language-bash}

~~~
   9 methane.pdb
  12 ethane.pdb
  15 propane.pdb
  20 cubane.pdb
  21 pentane.pdb
  30 octane.pdb
 107 total
~~~
{: .output}

然后我们可以通过另一个管道将该输出发送到“head”，这样整个管道就变成了：

~~~
$ wc -l *.pdb | sort -n | head -n 1
~~~
{: .language-bash}

~~~
   9  methane.pdb
~~~
{: .output}

这就像数学家嵌套函数，如 *log(3x)*
并说“三倍*x*的日志”。
在我们的案例中，
计算是'* .pdb'的行数的头部'。


最后几个命令中使用的重定向和管道如下所示：

![不同命令的重定向和管道：“wc -l *.pdb”将直接
输出到外壳。 “wc -l *.pdb > lengths”将直接输出到文件
“长度”。 “wc -l *.pdb | sort -n | head -n 1”将构建一个管道，其中
“wc”命令的输出是“sort”命令的输入，
“排序”命令是“头”命令的输入和输出
“head”命令被定向到 shell](../fig/redirects-and-pipes.svg)

> ## 管道组织命令
>
> 在我们的当前目录中，我们要找到 3 个文件数量最少的文件
> 线。 下面列出的哪个命令会起作用？
>
> 1. `wc -l * > sort -n > head -n 3`
> 2. `wc -l * | sort -n | head -n 1-3`
> 3. `wc -l * | head -n 3 | sort -n`
> 4. `wc -l * | sort -n | head -n 3`
>
> > ## 解决方案
> > 选项 4 是解决方案。
> > 管道字符`|`用于将一个命令的输出连接到
> > 另一个的输入。
> > `>` 用于将标准输出重定向到文件。
> > 在 `shell-lesson-data/molecules` 目录下试试吧！
> {: .solution}
{: .challenge}


## 旨在协同工作的工具
这种将程序链接在一起的想法是 Unix 如此成功的原因。
与其创建试图做许多不同事情的巨大程序，
Unix 程序员专注于创建许多简单的工具，每个工具都能很好地完成一项工作，
并且彼此配合得很好。
这种编程模型称为“管道和过滤器”。
我们已经看到了管道；
**filter** 是类似于 `wc` 或 `sort` 的程序
它将输入流转换为输出流。
几乎所有标准的 Unix 工具都可以这样工作：
除非另有说明，否则
他们从标准输入中读取，
用他们读过的东西做点什么，
并写入标准输出。

关键是任何从标准输入读取文本行的程序
并将文本行写入标准输出
也可以与其他所有以这种方式运行的程序结合使用。
您可以*并且应该*以这种方式编写程序
这样您和其他人就可以将这些程序放入管道中以增加其功能。


> ## 管道阅读理解
>
> 一个名为 `animals.txt` 的文件（在 `shell-lesson-data/data` 文件夹中）包含以下数据：
>
> ~~~
> 2012-11-05,deer
> 2012-11-05,rabbit
> 2012-11-05,raccoon
> 2012-11-06,rabbit
> 2012-11-06,deer
> 2012-11-06,fox
> 2012-11-07,rabbit
> 2012-11-07,bear
> ~~~
> {: .source}
>
> 什么文本通过每个管道以及下面管道中的最终重定向？
>
> ~~~
> $ cat animals.txt | head -n 5 | tail -n 3 | sort -r > final.txt
> ~~~
> {: .language-bash}
> 提示：一次建立一个命令来测试你的理解
> > ## 解决方案
> > `head` 命令从 `animals.txt` 中提取前 5 行。
> > 然后，使用 `tail` 命令从前 5 行中提取最后 3 行。
> > 使用 `sort -r` 命令，这 3 行以相反的顺序排序，最后，
> > 输出被重定向到文件`final.txt`。
> > 这个文件的内容可以通过执行`cat final.txt`来检查。
> > 该文件应包含以下几行：
> > ```
> > 2012-11-06,rabbit
> > 2012-11-06,deer
> > 2012-11-05,raccoon
> > ```
> > {: .source}
> {: .solution}
{: .challenge}

> ## 管道建设
>
> 对于上一个练习中的文件 `animals.txt`，考虑以下命令：
>
> ~~~
> $ cut -d , -f 2 animals.txt
> ~~~
> {: .language-bash}
>
> `cut` 命令用于删除或“剪切”文件中每一行的某些部分，
> 和 `cut` 期望用 <kbd>Tab</kbd> 字符将行分隔成列。
> 以这种方式使用的字符称为**分隔符**。
> 在上面的示例中，我们使用 `-d` 选项将逗号指定为分隔符。
> 我们还使用了 `-f` 选项来指定我们要提取第二个字段（列）。
> 这给出以下输出：
>
> ~~~
> deer
> rabbit
> raccoon
> rabbit
> deer
> fox
> rabbit
> bear
> ~~~
> {: .output}
>
> `uniq` 命令过滤掉文件中相邻的匹配行。
> 你怎么能扩展这个管道（使用 `uniq` 和另一个命令）来查找
> 找出文件包含的动物（其中没有任何重复
> 名称）？
>
> > ## Solution
> > ```
> > $ cut -d , -f 2 animals.txt | sort | uniq
> > ```
> > {: .language-bash}
> {: .solution}
{: .challenge}

> ## 哪个管道？
>
> 文件 `animals.txt` 包含 8 行数据，格式如下：
>
> ~~~
> 2012-11-05,deer
> 2012-11-05,rabbit
> 2012-11-05,raccoon
> 2012-11-06,rabbit
> ...
> ~~~
> {: .output}
>
> `uniq` 命令有一个 `-c` 选项，用于计算
> 一行在其输入中出现的次数。 假设你当前
> 目录是`shell-lesson-data/data/`，你会用什么命令来生成
> 一个表格，显示文件中每种动物的总数？
>
> 1.  `sort animals.txt | uniq -c`
> 2.  `sort -t, -k2,2 animals.txt | uniq -c`
> 3.  `cut -d, -f 2 animals.txt | uniq -c`
> 4.  `cut -d, -f 2 animals.txt | sort | uniq -c`
> 5.  `cut -d, -f 2 animals.txt | sort | uniq -c | wc -l`
>
> > ## 解决方案
> > 选项 4. 是正确答案。
> > 如果您难以理解原因，请尝试运行命令或子部分
> > 管道（确保您位于 `shell-lesson-data/data` 目录中）。
> {: .solution}
{: .challenge}

## Nelle 的管道：检查文件

Nelle 已经通过化验机运行了她的样本
并在前面描述的“north-pacific-gyre/2012-07-03”目录中创建了 17 个文件。
作为快速检查，从她的主目录开始，Nelle 键入：

~~~
$ cd north-pacific-gyre/2012-07-03
$ wc -l *.txt
~~~
{: .language-bash}

输出是 18 行，如下所示：

~~~
300 NENE01729A.txt
300 NENE01729B.txt
300 NENE01736A.txt
300 NENE01751A.txt
300 NENE01751B.txt
300 NENE01812A.txt
... ...
~~~
{: .output}

现在她输入这个：

~~~
$ wc -l *.txt | sort -n | head -n 5
~~~
{: .language-bash}

~~~
 240 NENE02018B.txt
 300 NENE01729A.txt
 300 NENE01729B.txt
 300 NENE01736A.txt
 300 NENE01751A.txt
~~~
{: .output}

哎呀：其中一个文件比其他文件短 60 行。
当她回去检查它时，
她看到她在星期一早上 8:00 做了那个化验 --- 某人
可能是在周末使用机器，
她忘了重置它。
在重新运行该示例之前，
她检查是否有任何文件有太多数据：

~~~
$ wc -l *.txt | sort -n | tail -n 5
~~~
{: .language-bash}

~~~
 300 NENE02040B.txt
 300 NENE02040Z.txt
 300 NENE02043A.txt
 300 NENE02043B.txt
5040 total
~~~
{: .output}

这些数字看起来不错——但是倒数第三行中的“Z”在做什么？
她的所有样品都应标记为“A”或“B”；
按照惯例，
她的实验室使用“Z”表示缺少信息的样本。
为了找到其他喜欢的人，她这样做：

~~~
$ ls *Z.txt
~~~
{: .language-bash}

~~~
NENE01971Z.txt    NENE02040Z.txt
~~~
{: .output}

果然，
当她检查笔记本电脑上的日志时，
这些样本中的任何一个都没有记录深度。
由于以其他方式获取信息为时已晚，
她必须从她的分析中排除这两个文件。
她可以使用“rm”删除它们，
但实际上她以后可能会做一些分析，深度无关紧要，
因此，她以后必须小心使用通配符表达式选择文件
`NENE*A.txt NENE*B.txt`。


> ## 删除不需要的文件
>
> 假设你想删除你处理过的数据文件，只保留
> 您的原始文件和处理脚本以节省存储空间。
> 原始文件以 `.dat` 结尾，处理后的文件以 `.txt` 结尾。
> 以下哪项会删除所有已处理的数据文件，
> 并且*仅*处理过的数据文件？
>
> 1. `rm ?.txt`
> 2. `rm *.txt`
> 3. `rm * .txt`
> 4. `rm *.*`
>
> > ## 解决方案
> > 1. 这将删除具有单字符名称的 `.txt` 文件
> > 2. 这是正确答案
> > 3. shell 会扩展 `*` 以匹配当前目录中的所有内容，
> >    所以该命令会尝试删除所有匹配的文件和一个额外的
> >    名为 `.txt` 的文件
> > 4. shell 将扩展 `*.*` 以匹配所有具有任何扩展名的文件，
> >    所以这个命令会删除所有文件
> {: .solution}
{: .challenge}

{% include links.md %}
