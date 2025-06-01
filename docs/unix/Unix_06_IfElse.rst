.. _Unix_06_IfElse:

=============  
第6节: 条件判断  
=============  

  
.. note::  

  主题: conditionals, if/else statements  

  命令: if/else, -e, ! -e, elif  



首先，我们将开始用 Unix 命令处理 fMRI 数据。点击 :ref:`这里 <fMRI_01_DataDownload>` 查看如何下载 Flanker 数据集、安装 FSL 以及什么是去颅骨处理。完成后，返回本教程。

前一节关于 for 循环的视频向我们展示了如何运行许多代码块，并在每次执行时做出细微调整。但如果我们只想在满足某些条件时才运行代码怎么办？我们可以用 **条件语句**（也叫 if-else 语句）来自动化这些决策：如果某个条件为真，则执行某些操作；否则，如果条件不为真，则执行其他操作。

例如，我们可能想检查解剖目录，看看解剖图像是否已经过去颅骨处理。如果还没有去颅骨处理，那么就进行去颅骨处理；如果已经去过颅骨，那么什么也不做。

导航到 sub-01 解剖目录。从命令行输入：

::

  if [[ -e sub-01_T1w_brain_f02.nii.gz ]]; then
  	echo “Skull-stripped brain exists”
  fi

与 for 循环一样，if-else 语句也有三个不同的部分。第一部分以“if”一词开头，然后评估或检查括号内的语句是真还是假。在括号内，-e 代表“检查此文件是否存在”。如果 anat_brain.nii.gz 存在，则语句继续执行条件语句主体中的代码。主体中可以有任意多的代码行。最后一行 fi - 或者说是反向拼写的“if” - 结束条件语句，然后继续执行之后列出的任何代码。

注意：if-else 语句的格式需要完全正确：例如，第一括号和 -e 之间需要有一个空格。如果格式不正确，您将收到错误提示。

请注意，如果您输入了上述条件语句，则不会发生任何事情。这是因为在这种情况下，语句被评估为 FALSE：该文件不存在，因此代码不会被执行。如果我们想在条件语句为假时做某事，我们需要添加另一个部分：ELSE 部分，如下所示：

::

	if [[ -e sub-01_T1w_brain_f02.nii.gz ]]; then
		echo “Skull-stripped brain exists”
	else
		echo “Skull-stripped brain does not exist”
	fi

这意味着如果该条件语句为真，则运行这段代码（高亮显示）。如果不为真，则运行这段代码（高亮显示）。

您可以使用多个条件语句为您的 if/else 语句提供更多灵活性。例如，假设我们想检查去颅骨处理的图像是否存在。如果不存在，则判断原始解剖图像是否存在。如果原始图像也不存在，则打印出既没有去颅骨处理的图像也没有原始解剖图像。这需要一个 elif 语句，代表“else, if”。

::

	if [[ -e sub-01_T1w_brain_f02.nii.gz ]]; then
		echo “Skull-stripped brain exists”
	elif [[ -e sub-01_T1w.nii.gz ]]; then
		echo “Original anatomical brain exists”
	else
		echo “Neither the skull-stripped nor the original brain exists”
	fi

您还可以使用其他所谓的逻辑表达式来评估语句的真假。例如，在括号内，您可以使用一对 && 来检查两个文件是否都存在：

::

	if [[ -e sub-01_T1w.nii.gz && -e sub-01_T1w_f02_brain.nii.gz ]]; then
		echo “Both files exist”
	else
		echo “One or more files do not exist”
	fi

或者，您可以使用一对竖线来检查一个文件或另一个文件是否存在：

::

	if [[ -e sub-01_T1w.nii.gz || -e sub-01_T1w_f02_brain.nii.gz ]]; then
		echo “At least one of the files exists”
	else
		echo “Neither of the files exists”
	fi

您还可以通过在 -e 选项前面加上感叹号来检查文件是否不存在：

::

	if [[ ! -e sub-01_T1w_f02_brain.nii.gz ]]; then
		echo “The skull-stripped brain doesn’t exist”
	else
		echo “The skull-stripped brain does exist”
	fi

现在，我们将演示如何将 for 循环与 if/else 语句结合使用。假设我们想检查被试 1、2 和 3 是否具有去颅骨处理的解剖图像。如果不存在，则使用 bet2 去除颅骨。导航到包含所有受试者的目录，然后运行以下代码：

::

	for i in sub-01 sub-02 sub-03; do
		cd ${i}/anat
		if [[ ! -e ${i}_T1w_f02_brain.nii.gz ]]; then
			echo “Skull-stripped brain doesn’t exist; stripping the brain with a fractional intensity threshold of 0.2”
			bet2 ${i}_T1w.nii.gz ${i}_T1w_f02_brain.nii.gz -f 0.2
		else
			echo “Skull-Stripped brain already exists; doing nothing”
		fi
		cd ../..
