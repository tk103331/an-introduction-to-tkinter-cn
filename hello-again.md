

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

> 注意：一些 Python 开发环境在运行 Tkinter 示例时遇到了问题。 问题通常是环境使用Tkinter本身，主环调用和退出调用与环境的期望相互作用。 其他环境可能会错误，如果你省略显式destroy调用。 如果示例的行为不符合预期，请检查您的开发环境的Tkinter特定文档。

Note: Some Python development environments have problems running Tkinter examples like this one. The problem is usually that the enviroment uses Tkinter itself, and the mainloop call and the quit calls interact with the environment’s expectations. Other environments may misbehave if you leave out the explicit destroy call. If the example doesn’t behave as expected, check for Tkinter-specific documentation for your development environment.

Details

This sample application is written as a class. The constructor (the __init__ method) is called with a parent widget (the master), to which it adds a number of child widgets. The constructor starts by creating a Frame widget. A frame is a simple container, and is in this case only used to hold the other two widgets.

class App:
    def __init__(self, master):
        frame = Frame(master)
        frame.pack()
The frame instance is stored in a local variable called frame. After creating the widget, we immediately call the pack method to make the frame visible.

We then create two Button widgets, as children to the frame.

 
self.button = Button(frame, text="QUIT", fg="red", command=frame.quit)
self.button.pack(side=LEFT)

self.hi_there = Button(frame, text="Hello", command=self.say_hi)
self.hi_there.pack(side=LEFT)
This time, we pass a number of options to the constructor, as keyword arguments. The first button is labelled “QUIT”, and is made red (fg is short for foreground). The second is labelled “Hello”. Both buttons also take a command option. This option specifies a function, or (as in this case) a bound method, which will be called when the button is clicked.

The button instances are stored in instance attributes. They are both packed, but this time with the side=LEFT argument. This means that they will be placed as far left as possible in the frame; the first button is placed at the frame’s left edge, and the second is placed just to the right of the first one (at the left edge of the remaining space in the frame, that is). By default, widgets are packed relative to their parent (which is master for the frame widget, and the frame itself for the buttons). If the side is not given, it defaults to TOP.

The “hello” button callback is given next. It simply prints a message to the console everytime the button is pressed:

def say_hi(self):
    print "hi there, everyone!"
Finally, we provide some script level code that creates a Tk root widget, and one instance of the App class using the root widget as its parent:

root = Tk()

app = App(root)

root.mainloop()
root.destroy()
The mainloop call enters the Tk event loop, in which the application will stay until the quit method is called (just click the QUIT button), or the window is closed.

The destroy call is only required if you run this example under certain development environments; it explicitly destroys the main window when the event loop is terminated. Some development environments won’t terminate the Python process unless this is done.

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