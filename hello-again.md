

**[上一页](hello-tkinter.md)**    **[下一页](tkinter-classes.md)**

----------

## Hello, Again ##

当你编写较大的程序时，通常最好将你的代码放在一个或多个类中。 以下示例改编自 Matt Conway 的 A Tkinter Life Preserver 中的“hello world”程序。

我们的第二个Tkinter程序

``` python
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

```
**运行这个例子**

运行此示例时，将显示以下窗口。

运行示例程序（在Windows 95机器上使用Tk 8.0）。

如果你点击右边的按钮，会在控制台打印文本“hi there，everyone！”；如果单击左边的按钮，程序停止。

> 注意：一些 Python 开发环境在运行 Tkinter 示例时遇到了问题。 问题通常是环境使用Tkinter本身，**mainloop** 调用和 **exit** 调用与环境的期望相互作用。 如果你省略显式的 **destroy** 调用，其他环境可能会错误。 如果示例的行为不符合预期，请检查您的开发环境的 Tkinter 特定文档。

**细节**

这个示例应用程序被写为类。 构造函数（**__ init__**方法）使用父控件（主控）调用，并向其添加了一些子控件。 构造函数从创建一个 **Frame** 组件开始。 **Frame** 组件是一个简单的容器，在这种情况下只用于保存其他两个小部件。

```python
class App:
    def __init__(self, master):
	    frame = Frame(master)
	    frame.pack()
```

**Frame** 实例存储在称为 **frame** 的局部变量中。 创建组件后，我们立即调用 **pack** 方法使**frame**可见。

然后我们创建两个 **Button** 组件，作为框架的子控件。

```python
self.button = Button(frame, text="QUIT", fg="red", command=frame.quit)
self.button.pack(side=LEFT)

self.hi_there = Button(frame, text="Hello", command=self.say_hi)
self.hi_there.pack(side=LEFT)
```

这次，我们向构造函数传递了一些选项作为关键字参数。 第一个按钮标记为“QUIT”，并且变为红色（fg是foreground(前景)的缩写。 第二个标记为“Hello”。 这两个按钮都有一个命令选项。 此选项指定一个函数，或（在这个例子中）一个绑定的方法，将在单击按钮时调用。

按钮实例存储在实例属性中。 它们都被打包，但这次使用了 side = LEFT 参数。 这意味着它们将被放置在框架中尽可能远的地方; 第一个按钮位于框架的左边缘，第二个按钮位于第一个按钮的右边（在框架中剩余空间的左边缘）。 默认情况下，窗口组件相对于其父窗口进行打包。 如果没有给出side参数，它默认为TOP。

接下来给出“hello”按钮回调。 它只是在每次按下按钮时向控制台打印一条消息：

```python
def say_hi(self):
	print "hi there, everyone!"
```

最后，我们提供一些脚本级代码，用于创建一个 Tk 根窗口组件，以及创建App类的实例并使用根窗口组件作为其父窗口：

```python
root = Tk()

app = App(root)

root.mainloop()
root.destroy()
```

调用 **mainloop** 进入Tk事件循环，在该循环中应用程序将一直保持到调用 **quit** 方法（只需单击QUIT按钮），或者窗口关闭。

只有在某些开发环境下运行此示例时，才需要调用 destroy; 它在事件循环终止时显式地销毁主窗口。 一些开发环境不会终止Python进程，除非这样做。

**关于组件的引用**

在第二个示例中，框架组件存储在名为frame的局部变量中，而按钮组件存储在两个实例属性中。 是不是有一个严重的问题隐藏在这里：当__ init__函数返回并且框架变量超出范围时会发生什么？

放轻松; 实际上没有必要保持对组件实例的引用。Tkinter自动维护一个组件树（通过每个组件实例的master属性和children属性），因此当应用程序的最后一个引用消失时，小部件不会消失; 它必须在这之前被显式地销毁（使用destroy方法）。 但是如果你想在窗口组件创建后做一些事情，你最好自己保持对窗口部件实例的引用。

请注意，如果您不需要保留对窗口小部件的引用，则可能很容易在一行中创建和打包它：

```python
Button(frame, text="Hello", command=self.hello).pack(side=LEFT)
```

不要存储此操作的结果; 你只会在尝试使用该值时感到失望（pack方法返回None）。 为了安全起见，最好始终将构造与打包分开：

```python
w = Button(frame, text="Hello", command=self.hello) 
w.pack(side=LEFT)
```

**关于组件的名称**

另一个让人困惑的原因是Tkinter关于小部件名称的概念，特别是对于那些有使用Tcl编程Tk的经验的人来说。在Tcl中，必须显式地命名每个小部件。例如，下面的Tcl命令创建了一个名为“ok”的按钮，作为名为“dialog”的组件的子控件（该组件又是根窗口“.”的子组件）。

``` tcl
button .dialog.ok
```

相应的Tkinter调用如下所示：

```python
ok = Button(dialog)
```

然而，在Tkinter的例子中，ok和dialog是对小部件实例的引用，而不是小部件的实际名称。由于Tk本身需要这些名称，Tkinter会自动为每个新的小部件分配一个唯一的名称。在上面的例子中，对话框名称可能类似“.1428748”，按钮可以命名为“.1428748.1432920”。如果要获取Tkinter小部件的全名，只需在小部件实例上使用str函数：


```
>>> print str(ok)
.1428748.1432920
```
（如果要打印某些内容，Python会自动使用str函数来查找要打印的内容。但是很明显，像“name=ok”这样的操作不能做到这一点，因此如果需要名称，请确保始终显式使用str）。

如果确实需要指定小部件的名称，可以在创建小部件时使用name选项。其中一个（而且很可能是唯一的）原因是如果您需要与用Tcl编写的代码进行接口。

在下面的示例中，生成的小部件名为“.dialog.ok”。（或者，如果你忘了给对话框命名，类似“.1428748.ok”）：

```python
ok = Button(dialog, name="ok")
```

为了避免与Tkinter的命名方案冲突，不要使用只包含数字的名称。还要注意，name是一个“creation only”选项；一旦创建了组件，就不能更改名称。

----------

**[上一页](hello-tkinter.md)**    **[下一页](tkinter-classes.md)**