.. _fMRI_04_Preprocessing:


=============
fMRI(FSL) 第四章: 数据预处理
=============


.. note::
  许多示例都是从 ``Flanker/sub-08`` 目录运行的；我建议您在阅读本章其余部分之前，使用终端导航到该目录。
  
   
概述
-------------

既然我们知道了我们的数据在哪里以及它是什么样子，我们将进行 fMRI 分析的第一步： **预处理**.

把预处理想象成清理图像。例如，当您用相机拍照时，您可以做几件事让图像看起来更好：

* 消除红眼；
* 增加色彩饱和度；
* 去除阴影。

.. figure:: Before_After_Editing.png

  我们用相机拍摄的照片可能会很暗、模糊或有噪点（左图）。通过增强对比度、减少模糊和增加亮度来编辑图像后，我们最终得到了一张更清晰、更明确的图片。

同样，当我们预处理 fMRI 数据时，我们正在清理我们每次  :ref:`TR <Repetition_Time>` 获取的三维图像. fMRI 图像不仅包含我们感兴趣的信号 —— 氧合血的变化 —— 还包含我们不感兴趣的波动，例如头部运动、随机漂移、呼吸和心跳。 我们将这些其他波动称为 **噪声**，因为我们希望将它们与我们感兴趣的信号分开。其中一些可以通过对它们进行建模从数据中回归出来（在关于模型拟合的章节中讨论），而其他的可以通过预处理来减少或去除。

要开始预处理 sub-08 的数据，请阅读以下关于每个步骤的描述。

.. toctree::
   :maxdepth: 1
   :caption: Preprocessing Steps

   Preprocessing/Skull_Stripping
   Preprocessing/FEAT_GUI
   Preprocessing/Motion_Correction
   Preprocessing/Slice_Timing_Correction
   Preprocessing/Smoothing
   Preprocessing/Registration_Normalization
   Preprocessing/Checking_Preprocessing
   Preprocessing/Checkpoint

---------

Video
*********

When you have finished all of the chapters, click `here <https://www.youtube.com/watch?v=VobRXk3ccNQ&list=PLIQIswOrUH6-rpwcmo2ewY2wi4Yoym9ft>`__ for a playlist containing all of the videos used to demonstrate the preprocessing steps.

.. note::
  Different software packages will do these steps in slightly different order - for example, FSL will normalize the statistical maps after model fitting. There are also analyses which omit certain steps - for example, some people who do multi-voxel pattern analyses don't smooth their data. In any case, the list above represents the most common steps that are performed on a typical dataset.
  
  
