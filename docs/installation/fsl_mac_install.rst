.. _fsl_mac_install:

概述
========


为了分析 fMRI 数据，您需要下载一个 fMRI 分析软件包。最受欢迎的三个软件包是：

* `SPM <https://www.fil.ion.ucl.ac.uk/spm/>`__ （统计参数映射，由伦敦大学学院创建）
* `FSL <https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/FSL>`__ （FMRIB 软件库，由牛津大学创建）
* `AFNI <https://afni.nimh.nih.gov/>`__ （功能性神经影像分析，由美国国立卫生研究院创建）


大多数实验室使用这三个软件包之一来分析 fMRI 数据。每个软件包都由专业团队维护，并且每隔几年都会更新一次。本课程将重点使用 FSL 分析一个在线数据集；未来将添加 SPM 和 AFNI 的教程。



安装 FSL
--------------

FSL 可以安装在不同的操作系统上，例如 Windows、Macintosh 和 Linux。Windows 版本需要一个虚拟机来运行 FSL。本课程假设您使用的是 Macintosh 或 Linux 操作系统，教程将在 iMac 桌面上录制。

有关在不同操作系统上安装 FSL 的说明可以在 `这里 <https://fsl.fmrib.ox.ac.uk/fsl/fslwiki/FslInstallation>`__ 找到。
这些说明非常详细，这里将不再重复；但对于使用 Macintosh 电脑的用户，可以在此处找到一个屏幕录制视频，指导您完成安装和设置： 
`FSL 安装视频演示 <https://youtu.be/E9FwDCYAto8?t=16>`__。


.. warning::

  您在运行 fslinstaller.py 时可能会遇到以下错误：[FAILED] [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed (_ssl.c:661)。在这种情况下，请尝试通过指定 python 命令的绝对路径来运行安装程序，例如：``/usr/bin/python fslinstaller.py``。


下一步
----------

如果您从未使用过命令行，请点击“下一步”按钮开始 Unix 教程。如果您对使用 Unix 感到熟悉，则可以跳到 :ref:`fMRI 简短课程 <fMRI_Intro>`。


