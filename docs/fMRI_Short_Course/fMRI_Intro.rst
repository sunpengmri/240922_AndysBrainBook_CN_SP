.. _fMRI_Intro:

============
功能磁共振概述
============

本课程将向您展示如何从头到尾分析一个 fMRI 数据集。我们将首先 **下载一个示例数据集** 并检查每个受试者的解剖和功能图像。然后我们将 **预处理** 数据，这将去除图像中的噪声并增强信号。一旦图像经过预处理，我们将创建一个模型，使用 :ref:`BOLD 信号 <BOLD_Response>` （一种神经活动的度量），来拟合我们fMRI图像中的信号。在 **模型拟合** 期间，我们将此模型与图像不同区域的信号进行比较。这种模型拟合是在不同条件下信号强度的一种度量 - 例如，我们可以获取实验条件 A 和 B 之间的信号差异，以查看哪个条件导致更大的 BOLD 响应。

一旦为每个受试者创建了模型，并估计了每个条件下的 BOLD 响应，我们就可以进行任何我们喜欢的组分析：配对 t 检验、组间 t 检验、交互作用等等。本课程的目标是计算两个条件之间的简单受试者内对比，并测试其在受试者之间是否显著。您还将学习如何创建类似于您在神经影像学杂志上看到的全脑分析图，以及如何进行感兴趣区域（ROI）分析。

.. figure:: Final_Map.png

    一个显示本课程中使用的数据的组间结果的图，以 z 统计量图表示。颜色越亮表示 z 分数越大。您将从预处理原始数据开始，以创建这样的统计图结束。
    

本课程旨在增强您处理 fMRI 数据的信心，提高您对 fMRI 分析基本术语的熟练程度，并帮助您在每个步骤中做出明智的选择。有些章节有练习，用于实践您所学的内容，并为您学习下一章做好准备。一旦您掌握了本课程的基础知识，您将能够将其应用于您选择的其他数据集。


.. note::
我们不会深入探讨 MRI 物理学。关于该主题的复习，我推荐 Huettel、Song 和 McCarthy 所著的《功能磁共振成像》（第 3 版）的第 1 - 5 章。也可以看看 Allen Elster 的优秀网站 `MRI Questions <http://mriquestions.com/index.html>`__ ，其中有关于 MRI 概念的漂亮图示.

