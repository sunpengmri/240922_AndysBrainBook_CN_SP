.. _Unix_09_AutomatingTheAnalysis:

第9节: 自动化分析
==================

----------------

概述
*********

当我们开始本教程时, 我们主要参考 :ref:`fMRI course <fMRI_01_DataDownload>`。 这里的大部分文本是从关于功能性磁共振成像的章节中借用的 :ref:`scripting <fMRI_06_Scripting>`, 它使用了我们所学的所有命令。以下部分将向您展示如何集成条件语句、for 循环和 sed，以便将单独的代码行集成到一个有用的脚本中。

运行脚本
**********

下面的代码旨在编辑和运行一个文件，该文件分析 Flanker 数据集中的每个被试。当你 `创建模板 <https://andysbrainbook.readthedocs.io/en/latest/fMRI_Short_Course/fMRI_06_Scripting.html#creating-the-template>`__ 后，将 design_run1.fsf 和 design_run2.fsf 文件移动到包含您的被试的目录中 (i.e., ``mv design*.fsf ..``, and then ``cd ..``)。 然后下载脚本 `run_1stLevel_Analysis.sh <https://github.com/andrewjahn/FSL_Scripts/blob/master/run_1stLevel_Analysis.sh>`__ 并将其移动到 Flanker 目录。该脚本内容已经在此打印：

::

  #!/bin/bash

  # Generate the subject list to make modifying this script
  # to run just a subset of subjects easier.

  for id in `seq -w 1 26` ; do
      subj="sub-$id"
      echo "===> Starting processing of $subj"
      echo
      cd $subj

          # If the brain mask doesn’t exist, create it
          if [ ! -f anat/${subj}_T1w_brain_f02.nii.gz ]; then
              bet2 anat/${subj}_T1w.nii.gz \
                  echo "Skull-stripped brain not found, using bet with a fractional intensity threshold of 0.2" \
                  anat/${subj}_T1w_brain_f02.nii.gz -f 0.2 #Note: This fractional intensity appears to work well for most of the subjects in the Flanker dataset. You may want to change it if you modify this script for your own study.
          fi

          # Copy the design files into the subject directory, and then
          # change “sub-08” to the current subject number
          cp ../design_run1.fsf .
          cp ../design_run2.fsf .

          # Note that we are using the | character to delimit the patterns
          # instead of the usual / character because there are / characters
          # in the pattern.
          sed -i '' "s|sub-08|${subj}|g" \
              design_run1.fsf
          sed -i '' "s|sub-08|${subj}|g" \
              design_run2.fsf

          # Now everything is set up to run feat
          echo "===> Starting feat for run 1"
          feat design_run1.fsf
          echo "===> Starting feat for run 2"
          feat design_run2.fsf
                  echo

      # Go back to the directory containing all of the subjects, and repeat the loop
      cd ..
  done

  echo

分析脚本
**********

让我们逐步了解此脚本的每个部分并描述其作用。


初始化 for 循环
^^^^^^^^^^

它以一个 shebang 和一些描述脚本确切作用的注释开头；然后使用反引号来包围 ``seq -w 1 26`` 以创建一个循环，该循环将对所有被试运行代码主体。这将拓展为 ``01、02、03... 26`` ，并在循环的每次迭代中更新分配给变量 id 的数字。

::

  #!/bin/bash

  # Generate the subject list to make modifying this script
  # to run just a subset of subjects easier.

  for id in `seq -w 1 26` ; do
      subj="sub-$id"
      echo "===> Starting processing of $subj"
      echo
      cd $subj

例如，此代码的第一个循环将字符串 ``sub-01`` 分配给变量 ``subj`` ，然后输出 "===> 开始处理 sub-01"。然后它将导航到 ``sub-01`` 目录。

条件语句：检查去颅骨后的解剖结构
^^^^^^^^^^

然后，脚本使用一个条件语句来检查去颅骨的解剖结构是否存在，如果不存在，则生成去颅骨的图像。

::

          # If the brain mask doesn’t exist, create it
          if [ ! -f anat/${subj}_T1w_brain_f02.nii.gz ]; then
              bet2 anat/${subj}_T1w.nii.gz \
                  echo "Skull-stripped brain not found, using bet with a fractional intensity threshold of 0.2" \
                  anat/${subj}_T1w_brain_f02.nii.gz -f 0.2 #Note: This fractional intensity appears to work well for most of the subjects in the Flanker dataset. You may want to change it if you modify this script for your own study.
          fi
      
      
编辑和运行模板文件
^^^^^^^^^^


然后，模板 design*.fsf 文件被编辑，以将字符串 ``sub-08`` 替换为当前主题的名称。*.fsf 文件使用 ``feat`` 命令运行，这就像从命令行运行 FEAT GUI 一样。在整个脚本中使用 Echo 命令让用户知道何时正在运行新的步骤。

::

          # Copy the design files into the subject directory, and then
          # change “sub-08” to the current subject number
          cp ../design_run1.fsf .
          cp ../design_run2.fsf .

          # Note that we are using the | character to delimit the patterns
          # instead of the usual / character because there are / characters
          # in the pattern.
          sed -i '' "s|sub-08|${subj}|g" \
              design_run1.fsf
          sed -i '' "s|sub-08|${subj}|g" \
              design_run2.fsf
              
        
位于 Flanker 主目录中的 design.fsf 文件被复制到当前主题的目录中。然后，Sed 将字符串 ``sub-08`` 替换为在循环中分配的 ``subj`` 的当前值。代码的最后一部分使用 ``feat`` 命令运行.fsf 文件，并打印到终端正在分析的是哪一次运行。
::

          # Now everything is set up to run feat
          echo "===> Starting feat for run 1"
          feat design_run1.fsf
          echo "===> Starting feat for run 2"
          feat design_run2.fsf
                  echo
                  
                  
您可以通过简单地输入 ``bash run_1stLevel_Analysis.sh`` 来运行脚本。当运行新步骤时， ``echo`` 命令将向终端打印文本，并且 HTML 页面将跟踪预处理和统计的进度。

----------

总结
***********

有 Unix 命令和概念。如果这是您第一次使用 Unix，这可能看起来很复杂；但通过练习，您将能够明白为什么脚本是这样组成的，以及相对较少的几行代码如何能够代表可能需要数十小时人工劳动的工作。

通过现在投入时间学习 Unix，您将能够使您的分析更快、更高效，并且更不容易出错。我也希望您在朝着将新技能应用于编写自己的分析脚本迈出第一步时变得更加自信。


----------