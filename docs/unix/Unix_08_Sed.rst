.. _Unix_08_Sed:

第8节: Sed 命令
================

.. note::

  主题: Stream editing, swapping
  
  命令: sed
  

概述
**********

到目前为止您所学的命令将允许您创建灵活的脚本，这些脚本可以适应许多不同的场景。您还将学习一个命令来完善您的 Unix 命令工具包： ``sed`` 命令。

Sed 是 "stream editor" 的缩写，因为 sed 的输入是文本流 - 与在 :ref:`previous chapter <Unix_03_ReadingTextFiles>` 中讨论的输入和输出流是相同的概念。 我们的目标是获取输入文本流并用另一个字符串替换一个字符串。与使用 for 循环执行类似过程相比，Sed 的优势在于 Sed 可以编辑文件，并且仅更改某些单词，同时覆盖文件并保持其余文本不变。

Sed 示例
**********

To see how sed works, create a text file called ``Hello.sh`` that contains the following line:

::

  #!/bin/bash
  
  echo "Hello, my name is Andy. Here is the name Andy again."
  

这只是一个运行一行代码的文本文件。如果您想将名字 Andy 换成 Bill，您将输入以下内容：

::

  sed "s|Andy|Bill|g" Hello.sh
  
注意，sed 命令分为三个部分：

1. 声明 sed 命令；
2. 要匹配并用另一个模式替换的模式，用引号括起来；
3. 要读入 sed 的文件（在本例中为 Hello.sh）

让我们关注模式部分。我更喜欢将这部分用双引号括起来，这样如果我在模式中包含一个变量，它将在 sed 命令运行之前被展开。这部分的第一部分是一个 “s”，意思是 “交换” 下面的字符串对。这对中的第一个是在文本文件中要搜索的内容，第二个是要替换为的内容。“g” 代表 “全局”，意思是将第一个单词的每个实例都替换为第二个单词。

如果您运行此命令，您应该看到以下输出：

::

  #!/bin/bash
  
  echo "Hello, my name is Bill. Here is the name Bill again."
  
如果您想将此输出重定向到一个新的文本文件，您将使用此代码：

::

  sed "s|Andy|Bill|g" Hello.sh > Hello_Bill.sh
  
一如既往，您可以随意命名输出文件。

就地编辑文件
**********

如果您想编辑文件并覆盖它，而不是将输出重定向到一个新文件，您可以使用 -i 和 -e 选项：

::

  sed -i -e "s|Andy|Bill|g" Hello.sh

-i 选项表示 “就地”，表示在单词交换后应覆盖文本文件。-e 选项用于使 -i 选项在 Macintosh 操作系统上工作；如果不包括它，sed 会抛出错误。


在 for 循环中使用 sed
***********

与其他命令一样，sed 可以与 for 循环和条件语句结合使用，以编写更复杂的代码。例如，假设我们想要创建一个模板文件的多个副本，并且在一系列名称中只更改其中的一个单词。让我们首先创建一个名为 ``Names.sh`` 的文件，其中包含以下内容：

::

  #!/bin/bash
  
  echo "Hi, my name is CHANGENAME."
  

在这里，CHANGENAME 是一个占位符；我把它全部大写以便突出显示，这在较大的文本文件中特别有用。现在我们可以使用 for 循环创建此文件的多个副本，将 CHANGENAME 替换为当前在循环中的任何名称：

::

  for name in Andy John Bill; do
    sed -i -e "s|CHANGENAME|${name}|g" Names.sh > ${name}_Names.sh
  done
  
在您输入此代码并运行它之前，思考一下会发生什么。想象列表中的项目将如何替换变量 **${name}** ，以及这将如何在 Names.sh 文件中与 CHANGENAME 进行交换。

现在运行代码。您得到预期的输出了吗？为什么或为什么没有？


----------

练习
*********

1. Sed 命令可以使用任何字符作为文件分隔符；例如，在 Hello.sh 脚本中尝试此代码：

::

  sed "s/name/last name/g" Hello.sh
  
现在用其他一些字符替换正斜杠。哪些分隔符（也称为定界符）看起来比其他的更好？为什么？什么时候正斜杠分隔符会有问题？


1. 您可以通过将最后的 ``g`` 更改为 ``d`` 在 sed 中删除一行。当使用 sed 删除一行时，您必须 1）删除初始的 ``s``，并且 2）仅使用正斜杠作为分隔符。例如，如果您想要删除包含字符串 “name” 的一行，您将输入：

::

  sed "/name/d" Hello.sh

明白了这一点，下载脚本 `Make FSL Timings <https://github.com/andrewjahn/FSL_Scripts/blob/master/make_FSL_Timings.sh>`__ ， 并使用 sed 删除任何包含字符串 ``run-1`` 的行。将输出与运行 sed 之前脚本中的内容进行比较。

---------



