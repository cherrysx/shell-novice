---
title: "循环"
teaching: 40
exercises: 10
questions:
- "如何对许多不同的文件执行相同的操作？"
objectives:
- "编写一个循环，将一个或多个命令分别应用于一组文件中的每个文件。"
- "跟踪循环执行期间循环变量的值。"
- "解释变量的名称和它的值之间的区别。"
- "解释为什么不应该在文件名中使用空格和一些标点符号。"
- "演示如何查看最近执行了哪些命令。"
- "重新运行最近执行的命令而不重新键入它们。"
keypoints:
- "`for` 循环对列表中的每件事重复一次命令。"
- "每个“for”循环都需要一个变量来引用它当前正在操作的东西。"
- "使用 `$name` 扩展变量（即获取其值）。 `${name}` 也可以使用。"
- "不要使用空格、引号或通配符，例如“*”或“?” 在文件名中，因为它使变量扩展复杂化。"
- "为文件提供易于与通配符模式匹配的一致名称，以便轻松选择它们进行循环。"
- "使用向上箭头键向上滚动以前的命令以编辑和重复它们。"
- "使用 <kbd>Ctrl</kbd>+<kbd>R</kbd> 搜索之前输入的命令。"
- "使用 `history` 显示最近的命令，使用 `![number]` 按编号重复命令。"
---

**循环**是一种编程结构，它允许我们重复一个命令或一组命令
对于列表中的每个项目。
因此，它们是通过自动化提高生产力的关键。
与通配符和制表符完成类似，使用循环也减少了
所需的打字量（从而减少打字错误的数量）。

假设我们有数百个名为“basilisk.dat”、“minotaur.dat”和
`独角兽.dat`。
对于这个例子，我们将使用只有三个示例文件的 `creatures` 目录，
但是这些原则可以同时应用于更多的文件。

这些文件的结构是相同的：通用名称、分类和更新日期是
在前三行中显示，DNA 序列在以下几行中。
让我们看一下文件：

```
$ head -n 5 basilisk.dat minotaur.dat unicorn.dat
```
{: .language-bash}

我们想打印出每个物种的分类，这是在第二个给出的
每个文件的行。
对于每个文件，我们需要执行命令“head -n 2”并将其通过管道传递给“tail -n 1”。
我们将使用循环来解决这个问题，但首先让我们看一下循环的一般形式：

```
for thing in list_of_things
do
    operation_using $thing    # Indentation within the loop is not required, but aids legibility
done
```
{: .language-bash}

我们可以将其应用于我们的示例，如下所示：

```
$ for filename in basilisk.dat minotaur.dat unicorn.dat
> do
>     head -n 2 $filename | tail -n 1
> done
```
{: .language-bash}

```
CLASSIFICATION: basiliscus vulgaris
CLASSIFICATION: bos hominus
CLASSIFICATION: equus monoceros
```
{: .output}


> ## 按照提示
>
> shell 提示符从 `$` 变为 `>` 并再次变回原来的样子
> 输入我们的循环。 第二个提示，`>`，是不同的提醒
> 我们还没有完成输入完整的命令。 分号，`;`，
> 可用于分隔写在一行上的两个命令。
{: .callout}

当 shell 看到关键字 `for` 时，
它知道为列表中的每个项目重复一次命令（或一组命令）。
每次循环运行（称为迭代）时，列表中的一个项目按顺序分配给
**变量**和循环内的命令被执行，然后继续
列表中的下一项。
在循环内部，
我们通过将`$`放在变量前面来调用变量的值。
`$` 告诉 shell 解释器处理
变量作为变量名并用它的值替换它的位置，
而不是将其视为文本或外部命令。

在此示例中，列表是三个文件名：“basilisk.dat”、“minotaur.dat”和“unicorn.dat”。
每次循环迭代时，它都会为变量`filename`分配一个文件名
并运行`head`命令。
第一次通过循环，
`$filename` 是 `basilisk.dat`。
解释器在 `basilisk.dat` 上运行命令 `head`
并将前两行通过管道传递给“tail”命令，
然后打印 `basilisk.dat` 的第二行。
对于第二次迭代，`$filename` 变为
`minotaur.dat`。这一次，shell 在 `minotaur.dat` 上运行 `head`
并将前两行通过管道传递给“tail”命令，
然后打印`minotaur.dat`的第二行。
对于第三次迭代，`$filename` 变为
`unicorn.dat`，所以 shell 在那个文件上运行 `head` 命令，
和输出的“尾巴”。
由于列表只有三个项目，shell 退出了 `for` 循环。

> ## 相同的符号，不同的含义
>
> 这里我们看到 `>` 被用作 shell 提示符，而 `>` 也是
> 用于重定向输出。
> 类似地，`$` 被用作 shell 提示符，但是，正如我们之前看到的，
> 它也用于要求 shell 获取变量的值。
>
> 如果 *shell* 打印 `>` 或 `$` 那么它希望你输入一些东西，
> 符号是提示符。
>
> 如果*你*自己键入`>`或`$`，这是你的指示
> shell 应该重定向输出或获取变量的值。
{: .callout}

使用变量时，它也是
可以将名称放入花括号中以清楚地分隔变量
name: `$filename` 等价于 `${filename}`，但不同于
`${file}name`。 您可能会在其他人的程序中找到这种表示法。

我们在这个循环中调用了变量`filename`
为了使人类读者更清楚其目的。
shell 本身并不关心变量的名称。
如果我们把这个循环写成：

~~~
$ for x in basilisk.dat minotaur.dat unicorn.dat
> do
>     head -n 2 $x | tail -n 1
> done
~~~
{: .language-bash}

or:

~~~
$ for temperature in basilisk.dat minotaur.dat unicorn.dat
> do
>     head -n 2 $temperature | tail -n 1
> done
~~~
{: .language-bash}

它的工作方式完全相同。
*不要这样做。*
程序只有在人们能够理解的情况下才有用，
如此无意义的名称（如“x”）或误导性名称（如“温度”）
增加程序不会像读者认为的那样做的可能性。

在上面的例子中，变量（`thing`、`filename`、`x` 和 `temperature`）
可以被赋予任何其他名称，只要它对双方都有意义
编写代码和阅读代码的人。

另请注意，循环可用于文件名以外的其他内容，例如数字列表
或数据子集。

> ## Write your own loop
>
> 你将如何编写一个循环来回显从 0 到 9 的所有 10 个数字？
>
> > ## Solution
> > 
> > ~~~
> > $ for loop_variable in 0 1 2 3 4 5 6 7 8 9
> > > do
> > >     echo $loop_variable
> > > done
> > ~~~
> > {: .language-bash}
> >
> > ```
> > 0
> > 1
> > 2
> > 3
> > 4
> > 5
> > 6
> > 7
> > 8
> > 9
> > ```
> > {: .output}
> {: .solution}
{: .challenge}

> ## 循环中的变量
>
> 这个练习是指 `shell-lesson-data/molecules` 目录。
> `ls` 给出以下输出：
>
> ~~~
> cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> ~~~
> {: .output}
>
> 以下代码的输出是什么？
>
> ~~~
> $ for datafile in *.pdb
> > do
> >     ls *.pdb
> > done
> ~~~
> {: .language-bash}
>
> 现在，以下代码的输出是什么？
>
> ~~~
> $ for datafile in *.pdb
> > do
> >     ls $datafile
> > done
> ~~~
> {: .language-bash}
>
> 为什么这两个循环给出不同的输出？
>
> > ## 解决方案
> > 第一个代码块在每次迭代时给出相同的输出
> > 循环。
> > Bash 在循环体中扩展通配符 `*.pdb`（以及
> > 在循环开始之前）匹配所有以 `.pdb` 结尾的文件
> > 然后使用 `ls` 列出它们。
> > 展开的循环如下所示：
> > ```
> > $ for datafile in cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> > > do
> > >     ls cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> > > done
> > ```
> > {: .language-bash}
> >
> > ```
> > cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> > cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> > cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> > cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> > cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> > cubane.pdb  ethane.pdb  methane.pdb  octane.pdb  pentane.pdb  propane.pdb
> > ```
> > {: .output}
> >
> > 第二个代码块列出了每次循环迭代的不同文件。
> > `datafile` 变量的值是使用 `$datafile` 评估的，
> > 然后使用 `ls` 列出。
> >
> > ```
> > cubane.pdb
> > ethane.pdb
> > methane.pdb
> > octane.pdb
> > pentane.pdb
> > propane.pdb
> > ```
> > {: .output}
> {: .solution}
{: .challenge}

> ## 限制文件集
>
> 在
> `shell-lesson-data/molecules` 目录？
>
> ~~~
> $ for filename in c*
> > do
> >     ls $filename
> > done
> ~~~
> {: .language-bash}
>
> 1. 没有列出任何文件。
> 2. 列出所有文件。
> 3. 仅列出 `cubane.pdb`、`octane.pdb` 和 `pentane.pdb`。
> 4. 仅列出 `cubane.pdb`。
>
> > ## 解决方案
> > 4 是正确答案。 `*` 匹配零个或多个字符，因此任何以
> > 字母 c，后跟零个或多个其他字符将被匹配。
> {: .solution}
>
> 输出与使用此命令有何不同？
>
> ~~~
> $ for filename in *c*
> > do
> >     ls $filename
> > done
> ~~~
> {: .language-bash}
>
> 1. 将列出相同的文件。
> 2. 这次列出了所有文件。
> 3. 这次没有列出任何文件。
> 4. 将列出文件 `cubane.pdb` 和 `octane.pdb`。
> 5. 只会列出文件 `octane.pdb`。
>
> > ## 解决方案
> > 4 是正确答案。 `*` 匹配零个或多个字符，因此文件名包含零个或多个
> > 字母 c 之前的字符和字母 c 之后的零个或多个字符将被匹配。
> {: .solution}
{: .challenge}

> ## 循环保存到文件 - 第一部分
>
> 在`shell-lesson-data/molecules` 目录下，这个循环的作用是什么？
>
> ~~~
> for alkanes in *.pdb
> do
>     echo $alkanes
>     cat $alkanes > alkanes.pdb
> done
> ~~~
> {: .language-bash}
>
> 1. 打印`cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb`, `pentane.pdb` and
>    `propane.pdb`，来自 `propane.pdb` 的文本将保存到一个名为 `alkanes.pdb` 的文件中。
> 2. 打印 `cubane.pdb`、`ethane.pdb` 和 `methane.pdb` 以及所有三个文件中的文本
>    将被连接并保存到一个名为 `alkanes.pdb` 的文件中。
> 3. 打印 `cubane.pdb`、`ethane.pdb`、`methane.pdb`、`octane.pdb` 和 `pentane.pdb`，
>    来自 `propane.pdb` 的文本将被保存到一个名为 `alkanes.pdb` 的文件中。
> 4. 以上都不是。
>
> > ## 解决方案
> > 1. 每个文件中的文本依次写入 `alkanes.pdb` 文件。
> > 但是，文件在每次循环迭代时都会被覆盖，因此 `alkanes.pdb` 的最终内容
> > 是 `propane.pdb` 文件中的文本。
> {: .solution}
{: .challenge}

> ## 循环保存到文件 - 第二部分
>
> 同样在 `shell-lesson-data/molecules` 目录中，
> 以下循环的输出是什么？
>
> ~~~
> for datafile in *.pdb
> do
>     cat $datafile >> all.pdb
> done
> ~~~
> {: .language-bash}
>
> 1. `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb`, 和
>    `pentane.pdb` 将被连接并保存到一个名为 `all.pdb` 的文件中。
> 2. 来自 `ethane.pdb` 的文本将被保存到一个名为 `all.pdb` 的文件中。
> 3. `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb`, `pentane.pdb` 的所有文本
>    和 `propane.pdb` 将被连接并保存到一个名为 `all.pdb` 的文件中。
> 4. `cubane.pdb`, `ethane.pdb`, `methane.pdb`, `octane.pdb`, `pentane.pdb` 的所有文本
>    和 `propane.pdb` 将被打印到屏幕上并保存到一个名为 `all.pdb` 的文件中。
>
> > ## 解决方案
> > 3 是正确答案。 `>>` 附加到一个文件，而不是用重定向覆盖它
> > 命令的输出。
> > 鉴于`cat` 命令的输出已被重定向，屏幕上不会打印任何内容。
> {: .solution}
{: .challenge}

让我们继续我们在 `shell-lesson-data/creatures` 目录中的示例。
这是一个稍微复杂的循环：

~~~
$ for filename in *.dat
> do
>     echo $filename
>     head -n 100 $filename | tail -n 20
> done
~~~
{: .language-bash}

shell 首先展开 `*.dat` 以创建它将处理的文件列表。
**循环体**
然后为每个文件执行两个命令。
第一个命令“echo”将其命令行参数打印到标准输出。
例如：

~~~
$ echo hello there
~~~
{: .language-bash}

输出:

~~~
hello there
~~~
{: .output}

在这种情况下，
由于 shell 将 `$filename` 扩展为文件名，
`echo $filename` 打印文件名。
请注意，我们不能这样写：

~~~
$ for filename in *.dat
> do
>     $filename
>     head -n 100 $filename | tail -n 20
> done
~~~
{: .language-bash}

因为那时第一次通过循环，
当 `$filename` 扩展为 `basilisk.dat` 时，shell 会尝试将 `basilisk.dat` 作为程序运行。
最后，
`head` 和 `tail` 组合选择第 81-100 行
从正在处理的任何文件
（假设文件至少有 100 行）。

> ## 名称中的空格
>
> 空格用于分隔列表的元素
> 我们将要循环。 如果这些元素之一
> 包含一个空格字符，我们需要用它包围它
> 引号，并对我们的循环变量做同样的事情。
> 假设我们的数据文件被命名为：
>
> ~~~
> red dragon.dat
> purple unicorn.dat
> ~~~
> {: .source}
>
> 要遍历这些文件，我们需要像这样添加双引号：
>
> ~~~
> $ for filename in "red dragon.dat" "purple unicorn.dat"
> > do
> >     head -n 100 "$filename" | tail -n 20
> > done
> ~~~
> {: .language-bash}
>
> 避免在文件名中使用空格（或其他特殊字符）更简单。
>
> 上面的文件是不存在的，所以如果我们运行上面的代码，`head` 命令将无法执行
> 找到它们，但是返回的错误消息将显示它所在的文件的名称
> 期待：
>
> ~~~
> head: cannot open ‘red dragon.dat’ for reading: No such file or directory
> head: cannot open ‘purple unicorn.dat’ for reading: No such file or directory
> ~~~
> {: .error}
>
> 尝试在上面的循环中删除 `$filename` 周围的引号以查看引号的效果
> 空格上的标记。 请注意，我们从 unicorn.dat 的循环命令中得到结果
> 当我们在 `creatures` 目录中运行这段代码时：
>
> ~~~
> head: cannot open ‘red’ for reading: No such file or directory
> head: cannot open ‘dragon.dat’ for reading: No such file or directory
> head: cannot open ‘purple’ for reading: No such file or directory
> CGGTACCGAA
> AAGGGTCGCG
> CAAGTGTTCC
> ...
> ~~~
> {: .output}
{: .callout}

我们想修改 `shell-lesson-data/creatures` 中的每个文件，但还要保存一个版本
原始文件，将副本命名为“original-basilisk.dat”和“original-unicorn.dat”。
我们不能使用：

~~~
$ cp *.dat original-*.dat
~~~
{: .language-bash}

因为这将扩展为：

~~~
$ cp basilisk.dat minotaur.dat unicorn.dat original-*.dat
~~~
{: .language-bash}

这不会备份我们的文件，而是会出现错误：

~~~
cp: target `original-*.dat' is not a directory
~~~
{: .error}

当 `cp` 接收到两个以上的输入时，就会出现这个问题。 发生这种情况时，它
期望最后一个输入是一个目录，它可以复制它传递的所有文件。
由于在 `creatures` 目录中没有名为 `original-*.dat` 的目录，我们得到一个
错误。

相反，我们可以使用循环：
~~~
$ for filename in *.dat
> do
>     cp $filename original-$filename
> done
~~~
{: .language-bash}

这个循环为每个文件名运行一次`cp`命令。
第一次，
当 `$filename` 扩展为 `basilisk.dat` 时，
外壳执行：

~~~
cp basilisk.dat original-basilisk.dat
~~~
{: .language-bash}

第二次，命令是：

~~~
cp minotaur.dat original-minotaur.dat
~~~
{: .language-bash}

第三次也是最后一次，命令是：

~~~
cp unicorn.dat original-unicorn.dat
~~~
{: .language-bash}

由于 `cp` 命令通常不会产生任何输出，因此很难检查
循环正在做正确的事情。
但是，我们之前学习了如何使用 `echo` 打印字符串，我们可以修改循环
使用 `echo` 打印我们的命令而不实际执行它们。
因此，我们可以检查在未修改的循环中*将*运行哪些命令。

下图
显示执行修改后的循环时会发生什么，并演示如何
明智地使用 `echo` 是一种很好的调试技术。

![for loop "for 文件名在 *.dat; do echo cp $filename original-$filename;
完成”将依次分配当前所有“*.dat”文件的名称
目录到变量“$filename”，然后执行命令。随着
当前目录中的文件“basilisk.dat”、“minotaur.dat”和“unicorn.dat”
循环将连续调用 echo 命令 3 次并打印 3 次
行：“cp baselisk.dat original-basilisk.dat”，然后是“cp minotaur.dat
original-minotaur.dat”，最后是“cp unicorn.dat”
原始独角兽.dat"](../fig/shell_script_for_loop_flow_chart.svg)

## Nelle 的管道：处理文件

Nelle 现在已准备好使用 `goostats.sh` 处理她的数据文件 ---
由她的主管编写的 shell 脚本。
这会从蛋白质样本文件中计算一些统计数据，并采用两个参数：

1. 输入文件（包含原始数据）
2. 输出文件（用于存储计算的统计信息）

因为她还在学习如何使用外壳，
她决定分阶段建立所需的命令。
她的第一步是确保她可以选择正确的输入文件 --- 记住，
这些名称以“A”或“B”结尾，而不是“Z”结尾。
Nelle 从她的主目录开始，键入：

~~~
$ cd north-pacific-gyre/2012-07-03
$ for datafile in NENE*A.txt NENE*B.txt
> do
>     echo $datafile
> done
~~~
{: .language-bash}

~~~
NENE01729A.txt
NENE01729B.txt
NENE01736A.txt
...
NENE02043A.txt
NENE02043B.txt
~~~
{: .output}

她的下一步是决定
`goostats.sh` 分析程序将创建的文件的名称。
用“stats”前缀每个输入文件的名称似乎很简单，
所以她修改了她的循环来做到这一点：

~~~
$ for datafile in NENE*A.txt NENE*B.txt
> do
>     echo $datafile stats-$datafile
> done
~~~
{: .language-bash}

~~~
NENE01729A.txt stats-NENE01729A.txt
NENE01729B.txt stats-NENE01729B.txt
NENE01736A.txt stats-NENE01736A.txt
...
NENE02043A.txt stats-NENE02043A.txt
NENE02043B.txt stats-NENE02043B.txt
~~~
{: .output}

她实际上还没有运行`goostats.sh`，
但现在她确信她可以选择正确的文件并生成正确的输出文件名。

一遍又一遍地输入命令变得乏味，
尽管，
Nelle担心犯错，
所以不是重新进入她的循环，
她按下 <kbd>↑</kbd>。
作为回应，
shell 在一行上重新显示整个循环
（使用分号分隔片段）：

~~~
$ for datafile in NENE*A.txt NENE*B.txt; do echo $datafile stats-$datafile; done
~~~
{: .language-bash}

使用左箭头键，
Nelle 备份并将命令 `echo` 更改为 `bash goostats.sh`：

~~~
$ for datafile in NENE*A.txt NENE*B.txt; do bash goostats.sh $datafile stats-$datafile; done
~~~
{: .language-bash}

当她按下 <kbd>Enter</kbd> 时，
shell 运行修改后的命令。
然而，似乎什么都没有发生——没有输出。
片刻之后，Nelle 意识到，因为她的脚本没有在屏幕上打印任何内容
再多，她也不知道它是否在跑，更不用说跑得有多快了。
她通过键入 <kbd>Ctrl</kbd>+<kbd>C</kbd> 来终止正在运行的命令，
使用 <kbd>↑</kbd> 重复命令，
并将其编辑为：

~~~
$ for datafile in NENE*A.txt NENE*B.txt; do echo $datafile;
bash goostats.sh $datafile stats-$datafile; done
~~~
{: .language-bash}

> ## 开头结尾
>
> 我们可以通过键入 <kbd>Ctrl</kbd>+<kbd>A</kbd> 移动到 shell 中的行首
> 并使用 <kbd>Ctrl</kbd>+<kbd>E</kbd> 到最后。
{: .callout}

当她现在运行她的程序时，
它每五秒左右产生一行输出：

~~~
NENE01729A.txt
NENE01729B.txt
NENE01736A.txt
...
~~~
{: .output}

1518次5秒，
除以 60，
告诉她她的脚本需要大约两个小时才能运行。
作为最后的检查，
她打开另一个终端窗口，
进入“北太平洋环流/2012-07-03”，
并使用`cat stats-NENE01729B.txt`
检查输出文件之一。
这看起来不错的样子，
所以她决定去喝杯咖啡，继续阅读。

> ## 懂历史的人可以选择重蹈覆辙
>
> 重复以前工作的另一种方法是使用 `history` 命令
> 获取最近执行的几百个命令的列表，以及
> 然后使用 `!123`（其中 '123' 替换为命令号）
> 重复这些命令之一。 例如，如果 Nelle 键入以下内容：
>
> ~~~
> $ history | tail -n 5
> ~~~
> {: .language-bash}
> ~~~
>   456  ls -l NENE0*.txt
>   457  rm stats-NENE01729B.txt.txt
>   458  bash goostats.sh NENE01729B.txt stats-NENE01729B.txt
>   459  ls -l NENE0*.txt
>   460  history
> ~~~
> {: .output}
>
> 然后她可以在 `NENE01729B.txt` 上重新运行`goostats.sh`，只需键入
> `!458`.
{: .callout}

> ## Other History Commands
>
> 还有许多其他快捷命令可用于查看历史记录。
>
> - <kbd>Ctrl</kbd>+<kbd>R</kbd> 进入历史搜索模式'reverse-i-search'并找到
> 您的历史记录中与您接下来输入的文本相匹配的最新命令。
> 按 <kbd>Ctrl</kbd>+<kbd>R</kbd> 一次或多次以搜索较早的匹配项。
> 然后您可以使用左右箭头键选择该行并进行编辑
> 然后点击 <kbd>Return</kbd> 运行命令。
> - `!!` 检索前面的命令
>（您可能会发现这比使用 <kbd>↑</kbd> 更方便）
> - `!$` 检索最后一个命令的最后一个单词。
> 这比您预期的更有用：之后
> `bash goostats.sh NENE01729B.txt stats-NENE01729B.txt`，可以输入
> `less !$` 查看文件 `stats-NENE01729B.txt`，即
> 比执行 <kbd>↑</kbd> 和编辑命令行更快。
{: .callout}

> ## 空跑
>
> 循环是一种同时做很多事情的方法 --- 或者在
> 一次，如果它做错了。 检查循环*会*做什么的一种方法
> 是“回显”它将运行的命令，而不是实际运行它们。
>
> 假设我们要预览以下循环将执行的命令
> 没有实际运行这些命令：
>
> ~~~
> $ for datafile in *.pdb
> > do
> >     cat $datafile >> all.pdb
> > done
> ~~~
> {: .language-bash}
>
> 下面的两个循环有什么区别，我们会选择哪一个
> 想运行吗？
>
> ~~~
> # Version 1
> $ for datafile in *.pdb
> > do
> >     echo cat $datafile >> all.pdb
> > done
> ~~~
> {: .language-bash}
>
> ~~~
> # Version 2
> $ for datafile in *.pdb
> > do
> >     echo "cat $datafile >> all.pdb"
> > done
> ~~~
> {: .language-bash}
>
> > ## 解决方案
> > 第二个版本是我们要运行的版本。
> > 这将打印以筛选包含在引号中的所有内容，扩展
> > 循环变量名，因为我们在它前面加上了美元符号。
> > 它也*不*修改或创建文件 `all.pdb`，作为 `>>`
> > 在字面上被视为字符串的一部分，而不是
> > 重定向指令。
> >
> > 第一个版本附加了命令 `echo cat $datafile` 的输出
> > 到文件，`all.pdb`。 该文件将仅包含列表；
> > `cat cubane.pdb`、`cat ethane.pdb`、`cat methane.pdb` 等。
> >
> > 自己试试这两个版本，看看输出！ 一定要打开
> > `all.pdb` 文件以查看其内容。
> {: .solution}
{: .challenge}

> ## 嵌套循环
>
> 假设我们要建立一个目录结构来组织
> 一些测量不同化合物反应速率常数的实验
> *和*不同的温度。 会是什么
> 以下代码的结果：
>
> ~~~
> $ for species in cubane ethane methane
> > do
> >     for temperature in 25 30 37 40
> >     do
> >         mkdir $species-$temperature
> >     done
> > done
> ~~~
> {: .language-bash}
>
> > ## 解决方案
> > 我们有一个嵌套循环，即包含在另一个循环中，所以对于每个物种
> > 在外循环中，内循环（嵌套循环）遍历列表
> > 温度，并为每个组合创建一个新目录。
> >
> > 尝试自己运行代码，看看创建了哪些目录！
> {: .solution}
{: .challenge}

{% include links.md %}
