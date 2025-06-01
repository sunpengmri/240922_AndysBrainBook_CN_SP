.. _Unix_08_Sed:

第8节: Sed 命令
================

.. note::

  涉及主题: 流编辑, 替换
  
  涉及命令: sed
  

概述
**********

到目前为止，您已经学习了如何创建灵活的脚本，这些脚本可以适应许多不同的场景。现在您将学习一个新的命令来完善您的 Unix 工具集：``sed`` 命令。

Sed 是 "stream editor"（流编辑器）的缩写，因为 sed 的输入是一个文本流——与之前章节 :ref:`<Unix_03_ReadingTextFiles>` 中讨论的输入和输出流概念相同。我们的目标是获取一个输入文本流，并将其中的一个字符串替换为另一个字符串。与使用 for 循环执行类似操作相比，sed 的优势在于它可以编辑文件，仅更改某些单词，同时覆盖文件并保留其余文本不变。

使用 Sed 的示例
**********

为了了解 sed 的工作原理，创建一个名为 ``Hello.sh`` 的文本文件，其中包含以下内容：

::

  #!/bin/bash
  
  echo "Hello, my name is Andy. Here is the name Andy again."
  

这只是一个运行一行代码的文本文件。如果您想将名字 Andy 替换为 Bill，可以输入以下命令：

::

  sed "s|Andy|Bill|g" Hello.sh
  
注意，sed 命令分为三个部分：

1. 声明 sed 命令；
2. 一个匹配模式和替换模式，用引号括起来；
3. 要读取到 sed 中的文件（在本例中是 Hello.sh)

我们重点关注模式部分。我更喜欢用双引号括住这一部分，这样如果模式中包含变量，它会在运行 sed 命令之前被展开。该部分的第一部分是一个 "s"，表示“替换”接下来的字符串对。字符串对的第一个是要在文本文件中搜索的内容，第二个是要替换的内容。"g" 表示“全局”，即将第一个单词的所有实例替换为第二个单词。

运行此命令后，您应该会看到以下输出：

::

  #!/bin/bash
  
  echo "Hello, my name is Bill. Here is the name Bill again."
  
如果您想将此输出重定向到一个新文本文件，可以使用以下代码：

::

  sed "s|Andy|Bill|g" Hello.sh > Hello_Bill.sh
  
如往常一样，您可以随意命名输出文件。

就地编辑文件
**********

如果您想编辑文件并覆盖它，而不是将输出重定向到新文件，可以使用 -i 和 -e 选项：

::

  sed -i -e "s|Andy|Bill|g" Hello.sh

-i 选项表示“就地”（in-place），意味着在替换单词后覆盖文本文件。-e 选项用于使 -i 选项在 Mac 操作系统上正常工作；如果不包含它，sed 会抛出错误。

结合 for 循环使用 sed
***********

与其他命令一样，sed 可以与 for 循环和条件语句结合使用，以编写更复杂的代码。例如，假设我们想创建模板文件的多个副本，并仅在名称列表中更改一个单词。首先创建一个名为 ``Names.sh`` 的文件，其中包含以下内容：

::

  #!/bin/bash
  
  echo "Hi, my name is CHANGENAME."
  

这里，CHANGENAME 是一个占位符；我将其全部大写以使其突出显示，这在较大的文本文件中特别有用。现在我们可以使用 for 循环创建该文件的多个副本，将 CHANGENAME 替换为当前循环中的名称：

::

  for name in Andy John Bill; do
    sed -i -e "s|CHANGENAME|${name}|g" Names.sh > ${name}_Names.sh
  done
  
在输入并运行此代码之前，思考一下会发生什么。想象列表中的项目如何替换变量 ${name}，以及如何将其与 Names.sh 文件中的 CHANGENAME 交换。

现在运行代码。您是否得到了预期的输出？

----------

练习
*********

1. sed 命令可以使用任何字符作为文件分隔符；例如，尝试以下代码与 Hello.sh 脚本一起使用：

::

  sed "s/name/last name/g" Hello.sh
  
现在将正斜杠替换为其他字符。哪些分隔符（也称为定界符）似乎比其他更好？为什么？在什么情况下正斜杠分隔符会有问题？


2. 您可以通过将最后的 ``g`` 更改为 ``d`` 来删除 sed 中的一行。在使用 sed 删除一行时，您必须 1）删除初始的 ``s``，并且 2）仅使用正斜杠作为分隔符。例如，如果您想删除包含字符串 "name" 的一行，可以输入：

::

  sed "/name/d" Hello.sh

了解这一点后，下载 `Make FSL Timings <https://github.com/andrewjahn/FSL_Scripts/blob/master/make_FSL_Timings.sh>`__ 脚本，并使用 sed 删除包含字符串 ``run-1`` 的任何行。将输出与运行 sed 之前的脚本进行比较。

---------



