.. _Skull_Stripping:

第一节: 大脑提取 ("skullstripping")
==============================

--------------------

由于 fMRI 研究专注于脑组织，我们的第一步是从图像中去除颅骨和非脑组织区域。FSL 有一个用于此的工具称为 **bet**，即脑提取工具。它是 FSL 图形用户界面（GUI）上列出的第一个按钮（如下图中的 “A” 所示）。如果您点击此按钮，将打开另一个窗口，允许您指定要进行颅骨剥离的输入图像以及对已进行颅骨剥离的输出图像进行标记（B），还有一个可扩展的子窗口，允许您指定高级选项（C）。

.. figure:: FSL_BET_GUI.png


.. note::
  对于 BET 和许多其他 FSL 工具，您需要指定输入图像和输出图像的标签：对输入图像执行某些操作（例如颅骨剥离），输出图像就是该操作的结果。通常，其他选项设置为对大多数数据集都适用的默认值，但如果您愿意，也可以覆盖它们。
  

在 ``sub-08`` 目录中打开 FSL GUI，点击 ``Input image`` 字段旁边的文件夹图标，然后导航到 ``anat`` 目录。选择文件 ``sub-08_T1w.nii.gz``，然后点击确定按钮。请注意， ``Output image``  字段会自动填入在您的输入图像后添加 ``brain`` 这个词，这是 FSL 的默认设置。如果您愿意，可以更改名称，但在本教程中，我们将保持原样。

现在点击窗口底部的 ``Go`` 按钮。您会看到终端中写入了一些文本，显示正在使用哪些命令来运行名为 ``bet2`` 的命令。花点时间看看图形用户界面（GUI）与终端是如何对应的 —— 稍后我们将利用这一点，通过 GUI 创建一个模板，然后在终端中修改它，以自动预处理我们数据集中的所有受试者。


查看数据
********

当终端显示 "Finished" 时， ``bet2`` 就完成了。由于您创建了一个新图像，您应该 **查看您的数据**，我们会在每个预处理步骤之后进行此操作。

.. note::
  新手们经常听到 “查看您的数据” 这句话，就像听到一句口头禅。如果不知道 **如何**查看自己的数据，这些话往好了说是毫无意义，往坏了说是一种虚假的安慰。本章中的每个预处理步骤之后都会有关于要查找什么的建议，以及什么是可以的、什么是有问题的具体示例 —— 以及如何处理。虽然我们无法涵盖所有可能的示例，但随着您经验的积累，您将对哪些图像质量好，哪些需要修复或删除形成自己的判断。
  

点击 GUI 底部的 ``FSLeyes`` 按钮。打开后，点击 ``File -> Add from File``，并按住 shift 键同时选择原始解剖图像和您刚刚创建的颅骨剥离图像。正如您在 :ref:`前一节 <fMRI_03_LookingAtTheData>` 的中看到的，您需要更改对比度以清楚地区分灰质和白质。


By loading both images you can compare the image before and after the skull was removed. In the ``Overlay List`` panel in the lower left corner of FSLeyes, click the "eye" icon to hide the corresponding image. For example, if you click on the eye icon next to ``sub-08_T1w``, the original T1 anatomical image will become invisible, and you will only see the skullstripped brain. If you click on the eye again, you will see the original T1. To make the differences between the brains more apparent, highlight the skullstripped image in the Overlay List panel, then change the contrast from ``Greyscale`` to ``Blue-Light blue``. The animation below shows you how to do this.
通过加载这两个图像，您可以比较去除颅骨前后的图像。在 FSLeyes 左下角的 ``Overlay List`` 面板中，点击 **眼睛** 图标来隐藏相应的图像。例如，如果您点击 ``sub-08_T1w`` 旁边的 **眼睛** 图标，原始的 T1 解剖图像将不可见，您将只看到去除颅骨的大脑。如果您再次点击该眼睛，您将看到原始的 T1 图像。为了使大脑之间的差异更明显，在叠加列表面板中突出显示去除颅骨的图像，然后将对比度从 ``Greyscale`` 更改为 ``Blue-Light blue``。下面的动画向您展示如何进行此操作。

用鼠标在图像上点击并观察哪里的脑组织过多或者去除的颅骨过少。记住，我们正在尝试创建一幅已经去除颅骨和面部、只留下大脑（例如，皮质、皮质下结构、脑干和小脑）的图像。

.. figure:: BET_Demonstration.gif

  这是一个如何使用 BET 检查颅骨剥离前后解剖图像的演示。请注意，在前额皮质，部分脑组织已被剥离。务必检查所有三个视图窗格以查看问题所在。

修复不良的颅骨剥离
***********
如果您对颅骨剥离不满意，您能怎么办？回想一下，BET 窗口包含我们如果愿意可以更改的选项。其中一个标记为 ``Fractional intensity threshold`` 的字段默认设置为 **0.5**。旁边的文本解释说，较小的值给出较大的大脑轮廓估计（反之，较大的值给出较小的大脑轮廓估计）。换句话说，如果我们认为去除的脑组织过多，我们应该将其设置为较小的数字，反之，如果我们认为去除的颅骨过少，则反之。

由于似乎 BET 去除了过多的脑组织，尝试将分数强度阈值降低到 0.2。同时确保将输出名称更改为有助于您记住所做操作的名称 - 例如， ``sub-08_T1w_brain_f02``。点击 ``Go`` 按钮重新运行颅骨剥离。

.. figure:: BET_f02_GUI.png


完成后，在 FSLeyes 中加载最新的颅骨剥离图像。点击原始解剖图像旁边的眼睛图标，也点击我们刚刚创建的最新颅骨剥离图像旁边的眼睛图标。注意更多的皮质得以保留的位置，特别是在前额皮质和顶叶皮质。您可能还注意到，此图像中更多的硬脑膜和一些颅骨仍然存在。一般来说，宁可保留过多的颅骨，也不要去除过多的皮质 —— 到处都是的一些颅骨碎片不会导致未来的预处理步骤失败（如标准化），但是一旦皮质被去除，就无法恢复了。


--------------

练习
***********

1. 将fractional intensity threshold更改为 0.1 并重新运行 BET，确保选择合适的输出名称以保持您的文件有条理。在 FSLeyes 中查看结果。以 0.9 的分数强度阈值重复这些步骤。您注意到了什么？什么似乎是一个好的阈值？

2. 在 FSLeyes 中为叠加图像试验不同的对比颜色，看看您最喜欢哪一个。使用缩放滑块（在放大镜图标旁边）聚焦在您认为剥离效果不好的区域。通过点击蒙太奇上方工具栏中的相机图标为蒙太奇（即所有三个视图窗格）拍照。


---------

.. Video
.. *******

.. To see a screencast demonstrating how to check your skullstripped image, click `here <https://youtu.be/VobRXk3ccNQ>`__. This may help you with the exercises above.
