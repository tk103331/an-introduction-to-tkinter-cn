**[上一页](tkinter-classes.md)**    **[下一页]( )**

----------

# 组件配置 #

为了控制组件的外观，通常使用选项而不是方法调用。典型的选项包括文本和颜色、大小、命令回调等。为了处理选项，所有核心部件都实现相同的配置接口：


配置接口(其中widgetclass是前面提到的组件类之一)：
``` python
widgetclass(master, option=value, …) => widget
```

使用给定的选项，创建这个组件类的实例，作为给定主组件(master)的子级。所有选项都有默认值，因此在最简单的情况下，您只需指定主组件(master)。如果你真的想要的话，你甚至可以省去它；Tkinter然后使用最近创建的根窗口作为主窗口。请注意，只有在创建组件时才能设置name选项。

``` python
cget(“option”) => string
```

返回选项的当前值。选项名和返回值都是字符串。要获得name选项，请改用 *str(widget)*。

``` python
config(option=value, …)
configure(option=value, …)
```

设置一个或多个选项（通过关键字参数提供）。

注意，有些选项的名称在Python中是保留字（class，from，…）。要使用这些参数作为关键字参数，只需在选项名称（class_，from_，…）后附加一个下划线。请注意，您不能使用这个方法设置name选项；它只能在创建组件时设置。

为了方便起见，组件还实现了部分字典接口。*__setitem\__* 方法映射到 *configure*，而*__getitem\__* 映射到 *cget*。因此，可以使用以下语法设置和查询选项：

``` python
value = widget["option"]
widget["option"] = value
```

请注意，每个赋值都会导致一个对Tk的调用。如果您希望更改多个选项，那么最好是通过一次config或configure调用来更改它们（就个人而言，我更喜欢以这种方式更改选项）。

以下字典方法也适用于组件：

``` python
keys() => list
```

返回可为此组件设置的所有选项的列表。*name* 选项不包括在这个列表中（任何途径都不能通过dictionary接口查询或修改它，所以这并不重要）。

## 向后兼容性 ##

python1.3 引入了关键字参数。在此之前，使用普通的 Python 字典将选项传递给小部件构造函数和configure方法。源代码可能会如下所示：

``` python 
self.button = Button(frame, {"text": "QUIT", "fg": "red", "command": frame.quit})
self.button.pack({"side": LEFT})
```

当然，关键字参数语法更优雅，更不容易出错。但是，为了与现有代码兼容，Tkinter仍然支持旧的语法。你不应该在新程序中使用这种语法，即使在某些情况下它可能很诱人。例如，如果您创建了一个需要将配置选项传递给其父类的自定义组件，您可能会得到类似以下内容：

``` python
def __init__(self, master, **kw):
    Canvas.__init__(self, master, kw) # kw is a dictionary
```

这在当前版本的Tkinter中工作得很好，但在将来的版本中可能无法工作。更一般的方法是使用 *apply* 函数：

``` python
def __init__(self, master, **kw):
    apply(Canvas.__init__, (self, master), kw)
```    

apply函数接受一个函数（在本例中是一个未绑定的方法）、一个带参数的元组（因为我们调用的是一个未绑定的方法，所以它必须包含self）和一个提供关键字参数的字典（可选）。


----------

**[上一页](tkinter-classes.md)**    **[下一页]( )**
