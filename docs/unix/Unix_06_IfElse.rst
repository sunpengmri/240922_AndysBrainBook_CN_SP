.. _Unix_06_IfElse:

===========================
第6节: 条件语句
===========================

.. note::

  主题: conditionals, if/else statements
  
  命令: if/else, -e, ! -e, elif
  
现在，我们将开始将 Unix 命令用于 fMRI 数据。点击 :ref:`here <fMRI_01_DataDownload>` 查看如何下载 Flanker 数据集、安装 FSL 以及什么是颅骨剥离的说明。当您完成后，请返回本教程。

前一个关于 for 循环的视频向我们展示了如何运行许多代码块，且每次执行之间有细微的变化。但如果我们只想在某些条件满足时运行代码呢？我们可以使用 **条件语句**，也称为 if - else 语句来自动做出这些决策：如果某个条件为真，那么就做某事；否则，如果条件不为真，就做其他的事。

例如，我们可能想要检查解剖学目录，查看解剖图像是否已经进行了颅骨剥离。如果还没有进行颅骨剥离，那么就进行颅骨剥离；如果已经剥离了，那么就什么都不做。

导航到 sub-01 解剖学目录。在命令行中，输入：

::

  if [[ -e sub-01_T1w_brain_f02.nii.gz ]]; then
  	echo “Skull-stripped brain exists”
  fi

像 for 循环一样，if - else 语句有三个不同的部分。第一部分以 “if” 开头，然后评估或检查括号中的语句是真还是假。在括号内，“-e” 表示 “检查此文件是否存在”。如果 anat_brain.nii.gz 存在，那么该语句将继续运行条件语句主体中的代码。在主体中，您可以根据需要拥有任意多行代码。最后一行 “fi”—— 或者说是 “if” 的倒写 —— 结束条件，并接着运行其后列出的任何代码。

注意：if - else 语句的格式必须准确：例如，在第一个括号和 -e 之间需要恰好一个空格。如果格式不是这样，将会发生错误。

请注意，如果您输入了上面的条件语句，什么都没有发生。那是因为在这种情况下，该语句被评估为假：该文件不存在，所以代码没有运行。如果我们想在条件语句为假时做些什么，我们需要添加另一个部分：一个 ELSE 部分，它看起来像这样：

::

	if [[ -e sub-01_T1w_brain_f02.nii.gz ]]; then
		echo “Skull-stripped brain exists”
	else
		echo “Skull-stripped brain does not exist”
	fi

这意味着如果这个条件语句为真，那么运行这段代码（突出显示）。如果不为真，那么运行这段代码（突出显示）。

您可以使用多个条件来使您的 if/else 语句更具灵活性。例如，假设我们要检查颅骨剥离图像是否存在。如果不存在，评估原始解剖图像是否存在。如果那个也不存在，那么打印出颅骨剥离图像和原始解剖图像都不存在。这需要一个 elif 语句，它代表 “否则，如果”。

::

	if [[ -e sub-01_T1w_brain_f02.nii.gz ]]; then
		echo “Skull-stripped brain exists”
	elif [[ -e sub-01_T1w.nii.gz ]]; then
		echo “Original anatomical brain exists”
	else
		echo “Neither the skull-stripped nor the original brain exists”
	fi

您可以使用其他所谓的逻辑表达式来评估语句是真还是假。例如，在括号内，您可以使用 **&&** 来检查两个文件是否都存在：

::

	if [[ -e sub-01_T1w.nii.gz && -e sub-01_T1w_f02_brain.nii.gz ]]; then
		echo “Both files exist”
	else
		echo “One or more files do not exist”
	fi

或者您可以使用 **||** 来检查是否存在一个文件或者另一个文件：

::

	if [[ -e sub-01_T1w.nii.gz || -e sub-01_T1w_f02_brain.nii.gz ]]; then
		echo “At least one of the files exists”
	else
		echo “Neither of the files exists”
	fi

您还可以通过在 -e 选项前放置一个感叹号来检查一个文件是否不存在：

::

	if [[ ! -e sub-01_T1w_f02_brain.nii.gz ]]; then
		echo “The skull-stripped brain doesn’t exist”
	else
		echo “The skull-stripped brain does exist”
	fi

现在，我们将以如何将 for 循环与 if/else 语句结合使用的演示作为结束。假设我们要检查受试者 1、2 和 3 是否有颅骨剥离的解剖图像。如果不存在，使用 bet2 进行颅骨剥离。导航到包含您所有受试者的目录，然后运行以下代码：

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
	done

这将导航到每个受试者的解剖学目录，并检查颅骨剥离图像是否存在。如果不存在，那么运行 bet 来对解剖图像进行颅骨剥离。echo 命令是可选的；我喜欢包含它们，以便用户知道当前正在运行什么命令。

在本教程中，我们涵盖了很多概念，但时间和实践会让您更熟悉如何将 for 循环和条件语句集成到您的代码中。下一个教程将向您展示如何将所有这些命令写入脚本，这会使您的代码更便于携带和更容易编辑。


----------


Exercises
*******



--------

