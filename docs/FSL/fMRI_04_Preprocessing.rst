.. _fMRI_04_Preprocessing:  


=============  
FSL教程 #4: 预处理  
=============  


.. note::  
  许多示例是在 ``Flanker/sub-08`` 目录中运行的；我建议在阅读本章之前，使用终端导航到该目录。  
  

概述  
-------------  

现在我们知道了数据的位置以及它的样子，我们将进行fMRI分析的第一步：**预处理**。  

可以将预处理看作是对图像的清理。比如，我们用相机拍摄的照片可能会很暗、模糊或有噪点（左图）。通过增强对比度、减少模糊和增加亮度编辑图像后，我们得到了一张更清晰、更定义明确的图片。  

类似地，当我们对fMRI数据进行预处理时，我们是在清理我们每次 :ref:`TR <Repetition_Time>` 获取的三维图像。fMRI体积不仅包含我们感兴趣的信号——氧合血液的变化——还包含我们不感兴趣的波动，例如头部运动、随机漂移、呼吸和心跳。我们将这些其他波动称为 **噪声** ，因为我们希望将它们与我们感兴趣的信号分离开来。其中一些噪声可以通过建模从数据中回归出去（将在模型拟合章节中讨论），而其他噪声可以通过预处理减少或去除。  

要开始预处理sub-08的数据，请阅读以下每个步骤的描述。  

.. toctree::  
   :maxdepth: 1  
   :caption: 预处理步骤  

   Preprocessing/Skull_Stripping  
   Preprocessing/FEAT_GUI  
   Preprocessing/Motion_Correction  
   Preprocessing/Slice_Timing_Correction  
   Preprocessing/Smoothing  
   Preprocessing/Registration_Normalization  
   Preprocessing/Checking_Preprocessing  
   Preprocessing/Checkpoint  

---------  
  

.. note::  
  不同的软件包会以略微不同的顺序执行这些步骤——例如，FSL会在模型拟合后对统计图进行归一化。此外，有些分析会省略某些步骤——例如，一些进行多体素模式分析（MVPA）的人不会对数据进行平滑处理。无论如何，上述列表代表了对典型数据集执行的最常见步骤。


