--- 
wordpress_url: http://luchanghong.com/rosemary/?p=140
wordpress_id: 140
title: QT designer and pyQT developping example
layout: post
date: 2012-04-23 15:13:00 +08:00
---
QT designer怎样把设计好的窗口程序转成Python代码呢？这是上周遇到的问题，解决之后总结分享一下。

开发环境：WIN 7  32bit

开发软件：python 2.6 + pyQT 4.8.0 + QT designer 4.8.1

一、用QT designer设计一个简单的留言框

QT designer操作很简单，拖一下左边的组件，多练习就知道每个组件到底是什么、怎么用了。

<a href="http://luchanghong.com/rosemary/wp-content/uploads/2012/04/QTdesigner.jpg"><img class="alignnone size-full wp-image-141" title="QTdesigner" src="http://luchanghong.com/rosemary/wp-content/uploads/2012/04/QTdesigner.jpg" alt="" width="398" height="280" /></a>

完成之后保存为message.ui，路径：E:\python\pyQT

二、把message.ui转成.py文件

QT designer自带的查看代码是C++代码，我需要的是Python，下面就要解决这个问题了。
<ol>
	<li>安装了pyQT之后，在Python目录下Lib\site-packages\pyQt4是这个包的路径，把D:\python26\Lib\site-packages\PyQt4\uic添加到环境变量</li>
	<li>在cmd窗口键入 pyuic -h命令看一下这个命令帮助提示。然后在message.ui目录下，执行命令：
pyuic message.ui -o message.py
执行成功，则当前目录（message.ui所在目录）下会生成message.py这个文件</li>
	<li>打开message.py这个文件，大致浏览一下，主要的就是一个类，接下来的问题就是怎么用？</li>
</ol>
三、使用message.py

在message.py目录下新建一个message_form.py，把message.py当作封装好的包import进来，示例代码如下：
<pre>[python]
import sys
import message
from PyQt4 import QtGui

class MessageForm(QtGui.QMainWindow, message.Ui_Form):
    def __init__(self, parent = None):
        super(MessageForm ,self).__init__(parent)
        self.setupUi(self)

if __name__ == '__main__':
    app = QtGui.QApplication(sys.argv)
    msg_form = MessageForm()
    msg_form.show()
    app.exec_()
[/python]</pre>
<pre>然后，我们执行message_form.py这个文件就OK了：python message_form.py</pre>
<pre><a href="http://luchanghong.com/rosemary/wp-content/uploads/2012/04/message_form.jpg"><img class="alignnone size-full wp-image-142" title="message_form" src="http://luchanghong.com/rosemary/wp-content/uploads/2012/04/message_form.jpg" alt="" width="402" height="281" /></a></pre>
