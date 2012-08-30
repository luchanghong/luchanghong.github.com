--- 
wordpress_url: http://luchanghong.com/rosemary/?p=153
wordpress_id: 153
title: !binary |
  cHlRVOWIm+W7uuS4gOS4qm1haW5XaW5kb3flubbmt7vliqDkuIroj5zljZXm
  oI9tZW51QmFy

layout: post
date: 2012-05-07 16:33:27 +08:00
---
五一给自己放了一个星期的假，去了一趟天堂——苏州&amp;杭州。回来继续学习pyQT，之前觉得学习方向有点错误，应该先搞熟了pyQT，然后再借助QTdesigner快速开发。但这也不是绝对的，目前我的学习方法是先写一个pyQT应用，遇到不会的就去查一下手册，或者从QTdesigner上面做类似的操作，然后看一下源码，再类比到Python中来。

一、pyQT创建一个mainWindow

创建一个最简单的window，没有任何内容。
<pre>[python]

import sys
from PyQt4 import QtCore,QtGui

class MyWindow(QtGui.QMainWindow):
    def __init__(self, parent = None):
        QtGui.QMainWindow.__init__(self, parent)

if __name__ == '__main__':
    app = QtGui.QApplication(sys.argv)
    myWindow = MyWindow()
    myWindow.show()
    sys.exit(app.exec_())

[/python]</pre>
<pre><a href="http://luchanghong.com/rosemary/wp-content/uploads/2012/05/mainwindow1.jpg"><img class="alignnone size-full wp-image-154" title="mainwindow1" src="http://luchanghong.com/rosemary/wp-content/uploads/2012/05/mainwindow1.jpg" alt="" width="220" height="136" /></a></pre>
<pre>我们还可以给这个mainWindow设置一些属性，如宽高、标题等。</pre>
<pre>二、设置menuBar</pre>
<ol>
	<li>定义一个menuBar</li>
	<li>定义一个menu</li>
	<li>定义一个子菜单action</li>
	<li>把上面的关联起来</li>
</ol>
这里要弄清楚menuBar、menu、action的关系，action绑定在menu上面，menu绑定在menuBar上面，这样多个menu和多个action就可以组成一个menuBar了，最后把menuBar绑定到mainWindow上。
<pre>[python]

import sys
from PyQt4 import QtCore,QtGui

class MyWindow(QtGui.QMainWindow):
    def __init__(self, parent = None):
        QtGui.QMainWindow.__init__(self, parent)

        self.resize(400, 300)
        self.setWindowTitle('my window')

        self.menuBar = QtGui.QMenuBar()
        self.menu = QtGui.QMenu()
        self.menu.setTitle('menu')
        self.action = QtGui.QAction(self)
        self.action.setText('action')

        self.menu.addAction(self.action)
        self.menuBar.addMenu(self.menu)
        self.setMenuBar(self.menuBar)

if __name__ == '__main__':
    app = QtGui.QApplication(sys.argv)
    myWindow = MyWindow()
    myWindow.show()
    sys.exit(app.exec_())

[/python]</pre>
<pre><a href="http://luchanghong.com/rosemary/wp-content/uploads/2012/05/mainwindow2.jpg"><img class="alignnone size-full wp-image-156" title="mainwindow2" src="http://luchanghong.com/rosemary/wp-content/uploads/2012/05/mainwindow2.jpg" alt="" width="420" height="336" /></a></pre>
三、添加多个菜单
类似上面的做发，继续添加menu，在menu上绑定action，就可以添加多个菜单了。
<pre>[python]
import sys
from PyQt4 import QtCore,QtGui

class MyWindow(QtGui.QMainWindow):
    def __init__(self, parent = None):
        QtGui.QMainWindow.__init__(self, parent)

        self.resize(400, 300)
        self.setWindowTitle('my window')

        self.menuBar = QtGui.QMenuBar()
        self.menu = QtGui.QMenu()
        self.menu.setTitle('menu')
        self.action = QtGui.QAction(self)
        self.action.setText('action')

        self.menu_2 = QtGui.QMenu()
        self.menu_2.setTitle('menu_2')
        self.action_2_1 = QtGui.QAction(self)
        self.action_2_1.setText('action_2_1')
        self.action_2_2 = QtGui.QAction(self)
        self.action_2_2.setText('action_2_2')

        self.menu.addAction(self.action)
        self.menu_2.addAction(self.action_2_1)
        self.menu_2.addAction(self.action_2_2)
        self.menuBar.addMenu(self.menu)
        self.menuBar.addMenu(self.menu_2)
        self.setMenuBar(self.menuBar)

if __name__ == '__main__':
    app = QtGui.QApplication(sys.argv)
    myWindow = MyWindow()
    myWindow.show()
    sys.exit(app.exec_())
[/python]</pre>
<pre><a href="http://luchanghong.com/rosemary/wp-content/uploads/2012/05/mainwindow3.jpg"><img class="alignnone size-full wp-image-155" title="mainwindow3" src="http://luchanghong.com/rosemary/wp-content/uploads/2012/05/mainwindow3.jpg" alt="" width="420" height="336" /></a></pre>
比较简单的菜单栏就出来了，做的复杂点可以添加上快捷键、分割线、选中样式等等。菜单做好了也就等于应用程序的功能规划基本完成，下面就是每个子菜单action触发事件了，下篇文章再分享吧。
