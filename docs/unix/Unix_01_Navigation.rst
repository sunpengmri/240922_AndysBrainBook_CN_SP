.. _Unix_01_Navigation:

===============================================
Unix Tutorial #1: 目录操作
===============================================

.. note::
    主题: 目录, 目录导航
    
    命令: pwd, cd, ls


概述
--------

像其他操作系统一样，Unix 使用目录树来组织文件夹和文件——也称为目录层次结构或目录结构。在层次结构的顶部有一个名为 ``root`` 的文件夹，写作正斜杠 (``/``)。 所有其他文件夹（也称为目录）都包含在 ``root`` 文件夹, 而这些文件夹又可以包含其他文件夹。

Think of the directory hierarchy as an upside-down tree: ``root`` is the base of the tree, and all of the other folders extend from it, just as branches extend from the trunk.

.. figure:: UnixTree.png

    Root, symbolized by a forward slash (``/``), is the highest level of the directory tree; it contains folders such as ``bin`` (which contains binaries, or Unix commands such as pwd, cd, ls, and so on), ``mnt`` (which shows any currently mounted drives, such as external hard drives), and ``Users``. These directories in turn contain other directories - for example, ``Users`` contains the folder ``andrew``, which in turn contains the ``Desktop``, ``Applications``, and ``Downloads`` directories. This is how folders and files are organized within a directory tree.
    

To navigate around your computer, you will need to know the commands ``pwd``, ``cd``, and ``ls``. ``pwd`` stands for “print working directory”; ``cd`` stands for “change directory”; and ``ls`` stands for “list”, as in “list the contents of the current directory.” This is analogous to pointing and clicking on a folder on your Desktop, and then seeing what’s inside. Note that in these tutorials, the words “folder” and “directory” are used interchangeably.

.. figure:: Desktop_Folder.png

    Navigation in Unix is the same thing as pointing and clicking in a typical graphical user interface. For example, if you have the folder "ExperimentFolder" on my Desktop, you can point and double-click to open it. You can do the same thing by typing ``cd ~/Desktop/ExperimentFolder`` in the Terminal and then typing ``ls`` to see what's in the directory.


Video
-----

Click `here <https://www.youtube.com/watch?v=TQqJD-v6glE&list=PLIQIswOrUH69xOiblvvEz5KBwWaNRMEUp&index=2>`__ to see a video overview of the commands cd, ls, and pwd - the basic commands you will need to navigate around your directory tree.


-------------

Exercises
---------

When you're done watching the video, try the following exercises:

1.  Type ``ls ~`` and note what it returns; then type ``ls ~/Desktop``. How are the outputs different? Why?

2.  Navigate to the Desktop by typing ``cd ~/Desktop``. Type ``pwd`` and note what the path is. Then create a new directory using the ``mkdir`` command, choosing a name for the directory on your own. Navigate into that new directory and think about how your current path has been updated. Does that match what you see from typing ``pwd`` from your new directory?

3.  Define the terms ``cd``, ``ls``, and ``pwd`` in your own words. 
