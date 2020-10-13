

**[上一页](whats-tkinter.md)**    **[下一页](hello-again.md)**

----------

## Hello, Tkinter ##

废话不多说，现在来看一些代码。

如你所知，每个教程都会从一个“hello world”类型示例开始。 在此概述中，我们也会向您展示这样的示例，而且展示两个。

首先，让我们看一个非常小的版本：

**我们的第一个Tkinter程序（文件：code/hello-tkinter.py）**
``` python
    from Tkinter import *
    
    root = Tk()
    
    w = Label(root, text="Hello, world!")
    w.pack()
    
    root.mainloop()
```
**运行这个示例**

要运行程序，像往常一样运行脚本：
``` shell
    $ python hello1.py
```
将出现以下窗口。

![](helloworld.gif)

要停止程序，只需关闭窗口。

**细节**

我们从导入 Tkinter 模块开始。 它包含与Tk工具包一起使用所需的所有的类、函数和其他一些东西。 在大多数情况下，您可以简单地将所有内容从Tkinter导入到模块的命名空间：
``` python
    from Tkinter import *
```
要初始化Tkinter，我们必须创建一个 Tk 根组件。 这是一个普通的窗口，有由窗口管理器提供的一个标题栏和其他装饰。 你应该为每个程序只创建一个根组件，并且必须先于任何其它窗口组件之前创建。
``` python
    root = Tk()
```
接下来，我们创建一个 Label 组件作为根窗口的子组件：
``` python
	w = Label(root, text="Hello, world!")
	w.pack()
```
Label 组件可以显示文本、图标、其他图像。 在这个示例中，我们使用 **text** 选项指定要显示的文本。

接下来，我们调用这个组件的 **pack** 方法。 这告诉它自己调整大小以适合给定的文本，并使其自身可见。 但是，在我们进入 Tkinter 的事件循环之前，该窗口不会显示出来：
``` python
	root.mainloop()
```
程序将停留在事件循环中，直到我们关闭窗口。 事件循环不仅处理来自用户（例如鼠标点击和按键）或窗口系统（例如重绘事件和窗口配置消息）的事件，它还处理由 Tkinter 本身排队的操作。 这些操作包括几何管理（通过 **pack** 方法排队）和显示更新。 这也意味着在进入主循环之前，应用程序窗口不会显示出来。

----------

**[上一页](whats-tkinter.md)**    **[下一页](hello-again.md)**