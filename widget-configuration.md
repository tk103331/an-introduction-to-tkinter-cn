**组件配置**

为了控制组件的外观，通常使用选项而不是方法调用。典型的选项包括文本和颜色、大小、命令回调等。为了处理选项，所有核心部件都实现相同的配置接口：



配置接口(其中widgetclass是前面提到的组件类之一)：
``` python
widgetclass(master, option=value, …) => widget
```

使用给定的选项，创建这个组件类的实例，作为给定主组件(master)的子级。所有选项都有默认值，因此在最简单的情况下，您只需指定主组件(master)。如果你真的想要的话，你甚至可以省去它；Tkinter然后使用最近创建的根窗口作为主窗口。请注意，只有在创建组件后才能设置name选项。

``` python
cget(“option”) => string
```

返回选项的当前值。选项名和返回值都是字符串。要获得name选项，请改用 *str(widget)*。

``` python
config(option=value, …)
configure(option=value, …)
```

设置一个或多个选项（通过关键字参数提供）。


Note that some options have names that are reserved words in Python (class, from, …). To use these as keyword arguments, simply append an underscore to the option name (class_, from_, …). Note that you cannot set the name option using this method; it can only be set when the widget is created.

For convenience, the widgets also implement a partial dictionary interface. The __setitem__ method maps to configure, while __getitem__ maps to cget. As a result, you can use the following syntax to set and query options:

value = widget["option"]
widget["option"] = value
Note that each assignment results in one call to Tk. If you wish to change multiple options, it is usually a better idea to change them with a single call to config or configure (personally, I prefer to always change options in that fashion).

The following dictionary method also works for widgets:

keys() => list

Return a list of all options that can be set for this widget. The name option is not included in this list (it cannot be queried or modified through the dictionary interface anyway, so this doesn’t really matter).

Backwards Compatibility
Keyword arguments were introduced in Python 1.3. Before that, options were passed to the widget constructors and configure methods using ordinary Python dictionaries. The source code could then look something like this:

 
self.button = Button(frame, {"text": "QUIT", "fg": "red", "command": frame.quit})
self.button.pack({"side": LEFT})
The keyword argument syntax is of course much more elegant, and less error prone. However, for compatibility with existing code, Tkinter still supports the older syntax. You shouldn’t use this syntax in new programs, even if it might be tempting in some cases. For example, if you create a custom widget which needs to pass configuration options along to its parent class, you may come up with something like:

def __init__(self, master, **kw):
    Canvas.__init__(self, master, kw) # kw is a dictionary
This works just fine with the current version of Tkinter, but it may not work with future versions. A more general approach is to use the apply function:

def __init__(self, master, **kw):
    apply(Canvas.__init__, (self, master), kw)
The apply function takes a function (an unbound method, in this case), a tuple with arguments (which must include self since we’re calling an unbound method), and optionally, a dictionary which provides the keyword arguments.