.. _Unix_09_AutomatingTheAnalysis:

第9节: 自动化分析
==================

----------------

概述
*********

当我们开始本教程时, 我们主要参考 :ref:`fMRI course <fMRI_01_DataDownload>`。 这里的大部分文本是从关于功能性磁共振成像的章节中借用的 :ref:`scripting <fMRI_06_Scripting>`, 它使用了我们所学的所有命令。以下部分将向您展示如何集成条件语句、for 循环和 sed，以便将单独的代码行集成到一个有用的脚本中。

运行脚本
**********

下面的代码旨在编辑并运行一个文件，该文件分析 Flanker 数据集中的每个受试者。一旦您 `创建了模板 <https://andysbrainbook.readthedocs.io/en/latest/fMRI_Short_Course/fMRI_06_Scripting.html#creating-the-template>`__，将 design_run1.fsf 和 design_run2.fsf 文件移动到包含您的受试者的目录中（即，``mv design*.fsf ..``，然后 ``cd ..``）。然后下载脚本 `run_1stLevel_Analysis.sh <https://github.com/andrewjahn/FSL_Scripts/blob/master/run_1stLevel_Analysis.sh>`__ 并将其移动到 Flanker 目录中。脚本内容如下：

::

  #!/bin/bash

  # 生成受试者列表，以便更容易修改此脚本
  # 以仅运行部分受试者。

  for id in `seq -w 1 26` ; do
      subj="sub-$id"
      echo "===> 开始处理 $subj"
      echo
      cd $subj

          # 如果脑掩膜不存在，则创建它
          if [ ! -f anat/${subj}_T1w_brain_f02.nii.gz ]; then
              bet2 anat/${subj}_T1w.nii.gz \
                  echo "未找到去颅骨的脑图像，使用 bet 并设置分数强度阈值为 0.2" \
                  anat/${subj}_T1w_brain_f02.nii.gz -f 0.2 #注意：此分数强度对 Flanker 数据集中的大多数受试者效果良好。如果您将此脚本用于自己的研究，可能需要更改它。
          fi

          # 将设计文件复制到受试者目录中，然后
          # 将“sub-08”更改为当前受试者编号
          cp ../design_run1.fsf .
          cp ../design_run2.fsf .

          # 注意，我们使用 | 字符作为模式的分隔符
          # 而不是通常的 / 字符，因为模式中包含 / 字符。
          sed -i '' "s|sub-08|${subj}|g" \
              design_run1.fsf
          sed -i '' "s|sub-08|${subj}|g" \
              design_run2.fsf

          # 现在一切都准备好运行 feat
          echo "===> 开始运行 feat，运行 1"
          feat design_run1.fsf
          echo "===> 开始运行 feat，运行 2"
          feat design_run2.fsf
                  echo

      # 返回包含所有受试者的目录，并重复循环
      cd ..
  done

  echo

分析脚本
**********

让我们逐步分析脚本的每个部分并描述其功能。

初始化 for 循环
^^^^^^^^^^

脚本以 shebang 和一些注释开始，描述了脚本的具体功能；然后使用反引号扩展 ``seq -w 1 26``，以创建一个循环，该循环将在所有受试者上运行代码主体。这将扩展为 ``01, 02, 03 ... 26``，并在循环的每次迭代中更新分配给变量 ``id`` 的编号。

::

  #!/bin/bash

  # 生成受试者列表，以便更容易修改此脚本
  # 以仅运行部分受试者。

  for id in `seq -w 1 26` ; do
      subj="sub-$id"
      echo "===> 开始处理 $subj"
      echo
      cd $subj

例如，此代码的第一个循环将字符串 ``sub-01`` 分配给变量 ``subj`` ，然后输出 "===> 开始处理 sub-01"。然后它将导航到 ``sub-01`` 目录。

例如，此代码的第一次循环将字符串 ``sub-01`` 分配给变量 ``subj``，然后输出 "===> 开始处理 sub-01"。接着，它将导航到 ``sub-01`` 目录。

检查去颅骨解剖图像的条件语句
^^^^^^^^^^

脚本随后使用条件语句检查去颅骨解剖图像是否存在，如果不存在，则生成去颅骨图像。

::

          # 如果脑掩膜不存在，则创建它
          if [ ! -f anat/${subj}_T1w_brain_f02.nii.gz ]; then
              bet2 anat/${subj}_T1w.nii.gz \
                  echo "未找到去颅骨的脑图像，使用 bet 并设置分数强度阈值为 0.2" \
                  anat/${subj}_T1w_brain_f02.nii.gz -f 0.2 #注意：此分数强度对 Flanker 数据集中的大多数受试者效果良好。如果您将此脚本用于自己的研究，可能需要更改它。
          fi
      

编辑并运行模板文件
^^^^^^^^^^

然后，模板 design*.fsf 文件被编辑，以将字符串 ``sub-08`` 替换为当前受试者的名称。*.fsf 文件通过 ``feat`` 命令运行，这类似于从命令行运行 FEAT GUI。脚本中使用了 echo 命令，向用户显示每个新步骤的运行情况。

::

          # 将设计文件复制到受试者目录中，然后
          # 将“sub-08”更改为当前受试者编号
          cp ../design_run1.fsf .
          cp ../design_run2.fsf .

          # 注意，我们使用 | 字符作为模式的分隔符
          # 而不是通常的 / 字符，因为模式中包含 / 字符。
          sed -i '' "s|sub-08|${subj}|g" \
              design_run1.fsf
          sed -i '' "s|sub-08|${subj}|g" \
              design_run2.fsf
              
           
设计.fsf 文件位于主 Flanker 目录中，被复制到当前受试者的目录中。Sed 随后将字符串 ``sub-08`` 替换为循环中分配给 ``subj`` 的当前值。代码的最后部分通过 ``feat`` 命令运行 .fsf 文件，并在终端中打印正在分析的运行。

::

          # 现在一切都准备好运行 feat
          echo "===> 开始运行 feat，运行 1"
          feat design_run1.fsf
          echo "===> 开始运行 feat，运行 2"
          feat design_run2.fsf
                  echo
                  
                  
您可以通过简单地输入 ``bash run_1stLevel_Analysis.sh`` 来运行脚本。echo 命令将在运行每个新步骤时向终端打印文本，而 HTML 页面将跟踪预处理和统计的进度。

----------

总结
***********

到目前为止，您已经学习了运行 fMRI 分析脚本所需的所有 Unix 命令和概念。如果这是您第一次使用 Unix，这可能看起来很复杂；但通过练习，您将能够理解脚本为何以这种方式组成，以及如何通过相对较少的代码行表示可能需要数十小时人工劳动的工作。

通过现在花时间学习 Unix，您将能够使分析更快、更高效且更少出错。我也希望，您已经更有信心迈出第一步，将新技能应用于编写您自己的分析脚本。



