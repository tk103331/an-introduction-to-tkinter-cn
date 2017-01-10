

**[上一页](whats-tkinter.md)**    **下一页**

----------

## Hello, Again ##

当你编写较大的程序时，通常最好将你的代码放在一个或多个类中。 以下示例改编自 Matt Conway 的 A Tkinter Life Preserver 中的“hello world”程序。

我们的第二个Tkinter程序

from Tkinter import *

	class App:
	
	    def __init__(self, master):
	
	        frame = Frame(master)
	        frame.pack()
	
	        self.button = Button(
	            frame, text="QUIT", fg="red", command=frame.quit
	            )
	        self.button.pack(side=LEFT)
	
	        self.hi_there = Button(frame, text="Hello", command=self.say_hi)
	        self.hi_there.pack(side=LEFT)
	
	    def say_hi(self):
	        print "hi there, everyone!"
	
	root = Tk()
	
	app = App(root)
	
	root.mainloop()
	root.destroy() # optional; see description below


**运行这个例子**

运行此示例时，将显示以下窗口。

运行示例程序（在Windows 95机器上使用Tk 8.0）。

如果你点击右边的按钮，会在控制台打印文本“hi there，everyone！”；如果单击左边的按钮，程序停止。

> 注意：一些 Python 开发环境在运行 Tkinter 示例时遇到了问题。 问题通常是环境使用Tkinter本身，**mainloop** 调用和 **exit** 调用与环境的期望相互作用。 如果你省略显式的 **destroy** 调用，其他环境可能会错误。 如果示例的行为不符合预期，请检查您的开发环境的 Tkinter 特定文档。

**细节**

这个示例应用程序被写为类。 构造函数（**__ init__**方法）使用父控件（主控）调用，并向其添加了一些子控件。 构造函数从创建一个 **Frame** 组件开始。 **Frame** 组件是一个简单的容器，在这种情况下只用于保存其他两个小部件。

    class App:
	    def __init__(self, master):
		    frame = Frame(master)
		    frame.pack()

**Frame** 实例存储在称为 **frame** 的局部变量中。 创建组件后，我们立即调用 **pack** 方法使**frame**可见。

然后我们创建两个 **Button** 组件，作为框架的子控件。
 
    self.button = Button(frame, text="QUIT", fg="red", command=frame.quit)
    self.button.pack(side=LEFT)
    
    self.hi_there = Button(frame, text="Hello", command=self.say_hi)
    self.hi_there.pack(side=LEFT)

这次，我们向构造函数传递了一些选项作为关键字参数。 第一个按钮标记为“QUIT”，并且变为红色（fg是foreground(前景)的缩写。 第二个标记为“Hello”。 这两个按钮都有一个命令选项。 此选项指定一个函数，或（在这个例子中）一个绑定的方法，将在单击按钮时调用。

按钮实例存储在实例属性中。 它们都被打包，但这次使用了 side = LEFT 参数。 这意味着它们将被放置在框架中尽可能远的地方; 第一个按钮位于框架的左边缘，第二个按钮位于第一个按钮的右边（在框架中剩余空间的左边缘）。 默认情况下，窗口组件相对于其父窗口进行打包。 如果没有给出side参数，它默认为TOP。

接下来给出“hello”按钮回调。 它只是在每次按下按钮时向控制台打印一条消息：

    def say_hi(self):
    	print "hi there, everyone!"

最后，我们提供一些脚本级代码，用于创建一个 Tk 根窗口组件，以及创建App类的实例并使用根窗口组件作为其父窗口：

	root = Tk()
	
	app = App(root)
	
	root.mainloop()
	root.destroy()

调用 **mainloop** 进入Tk事件循环，在该循环中应用程序将一直保持到调用 **quit** 方法（只需单击QUIT按钮），或者窗口关闭。

只有在某些开发环境下运行此示例时，才需要调用 destroy; 它在事件循环终止时显式地销毁主窗口。 一些开发环境不会终止Python进程，除非这样做。

More on widget references

In the second example, the frame widget is stored in a local variable named frame, while the button widgets are stored in two instance attributes. Isn’t there a serious problem hidden in here: what happens when the __init__ function returns and the frame variable goes out of scope?

Just relax; there’s actually no need to keep a reference to the widget instance. Tkinter automatically maintains a widget tree (via the master and children attributes of each widget instance), so a widget won’t disappear when the application’s last reference goes away; it must be explicitly destroyed before this happens (using the destroy method). But if you wish to do something with the widget after it has been created, you better keep a reference to the widget instance yourself.

Note that if you don’t need to keep a reference to a widget, it might be tempting to create and pack it on a single line:

 
Button(frame, text="Hello", command=self.hello).pack(side=LEFT)
Don’t store the result from this operation; you’ll only get disappointed when you try to use that value (the pack method returns None). To be on the safe side, it might be better to always separate construction from packing:

	w = Button(frame, text="Hello", command=self.hello) 
	w.pack(side=LEFT)
More on widget names

Another source of confusion, especially for those who have some experience of programming Tk using Tcl, is Tkinter’s notion of the widget name. In Tcl, you must explicitly name each widget. For example, the following Tcl command creates a Button named “ok”, as a child to a widget named “dialog” (which in turn is a child of the root window, “.”).

button .dialog.ok
The corresponding Tkinter call would look like:

	ok = Button(dialog)
However, in the Tkinter case, ok and dialog are references to widget instances, not the actual names of the widgets. Since Tk itself needs the names, Tkinter automatically assigns a unique name to each new widget. In the above case, the dialog name is probably something like “.1428748,” and the button could be named “.1428748.1432920”. If you wish to get the full name of a Tkinter widget, simply use the str function on the widget instance:

>>> print str(ok)
.1428748.1432920
(if you print something, Python automatically uses the str function to find out what to print. But obviously, an operation like “name = ok” won’t do the that, so make sure always to explicitly use str if you need the name).

If you really need to specify the name of a widget, you can use the name option when you create the widget. One (and most likely the only) reason for this is if you need to interface with code written in Tcl.

In the following example, the resulting widget is named “.dialog.ok” (or, if you forgot to name the dialog, something like “.1428748.ok”):

	ok = Button(dialog, name="ok")
To avoid conflicts with Tkinter’s naming scheme, don’t use names which only contain digits. Also note that name is a “creation only” option; you cannot change the name once you’ve created the widget.