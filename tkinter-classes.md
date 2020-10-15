## Tkinter 类 ##

**组件类**

Tkinter 支持15个核心组件：

- 按钮 Button

    一个简单的按钮，用于执行命令或其他操作。

- 画布 Canvas

    结构化图形。 此组件可用于绘制图形、创建图形编辑器，以及实现自定义组件。

- 复选按钮 Checkbutton

    表示可以有两个不同值的变量。 单击按钮可以在两个值之间切换。

- 输入 Entry

    文本输入字段。

- 框架 Frame

    一个容器组件。 框架可以有边框和背景，用于在创建应用程序或对话框布局时对其他窗口小部件进行分组。

- 标签 Label

    显示文本或图像。

- 列表框 Listbox

    显示备选项列表。 列表框可以配置为获取单选按钮或清单行为。

- 菜单 Menu

    菜单窗格。 用于实现下拉菜单和弹出菜单。

- 菜单按钮 Menubutton

    用于实现下拉菜单。

- 信息 Message

    显示文本。 类似于标签窗口小部件，但可以自动使用给定的宽度或宽高比包装文本。

- 单选按钮 Radiobutton

    表示可以具有多个值之一的变量。 单击按钮将变量设置为该值，并清除与同一变量关联的所有其他单选按钮。

- 尺度 Scale

    允许您通过拖动“滑块”设置数值。

- 滚动条 Scrollbar

    用于与画布、输入框、列表框和文本组件一起使用的标准滚动条。

- 文本 Text

    显示格式化文本。 允许您显示和编辑具有各种样式和属性的文本。 还支持嵌入式图像和窗口。

- 顶级窗口 Toplevel

    显示为单独的顶级窗口的容器组件。

在Python 2.3（Tk 8.4）中，添加了以下组件：

- 标签框架 LabelFrame

    Frame 组件的一个变体，可以绘制边框和标题。

- 分栏窗口 PanedWindow

    一个容器组件，用于在可调整大小的窗格中组织子组件。

- 旋转框 Spinbox

    一个Entry组件的变体，用于从范围或有序集中选择值。

Also note that there’s no widget class hierarchy in Tkinter; all widget classes are siblings in the inheritance tree.

All these widgets provide the Misc and geometry management methods, the configuration management methods, and additional methods defined by the widget itself. In addition, the Toplevel class also provides the window manager interface. This means that a typical widget class provides some 150 methods.

Mixins

The Tkinter module provides classes corresponding to the various widget types in Tk, and a number of mixin and other helper classes (a mixin is a class designed to be combined with other classes using multiple inheritance). When you use Tkinter, you should never access the mixin classes directly.

Implementation mixins

The Misc class is used as a mixin by the root window and widget classes. It provides a large number of Tk and window related services, which are thus available for all Tkinter core widgets. This is done by delegation; the widget simply forwards the request to the appropriate internal object.

The Wm class is used as a mixin by the root window and Toplevel widget classes. It provides window manager services, also by delegation.

Using delegation like this simplifies your application code: once you have a widget, you can access all parts of Tkinter using methods on the widget instance.

Geometry mixins

The Grid, Pack, and Place classes are used as mixins by the widget classes. They provide access to the various geometry managers, also via delegation.

Grid
The grid geometry manager allows you to create table-like layouts, by organizing the widgets in a 2-dimensional grid. To use this geometry manager, use the grid method.
Pack
The pack geometry manager lets you create a layout by “packing” the widgets into a parent widget, by treating them as rectangular blocks placed in a frame. To use this geometry manager for a widget, use the pack method on that widget to set things up.
Place
The place geometry manager lets you explicitly place a widget in a given position. To use this geometry manager, use the place method.
Widget configuration management

The Widget class mixes the Misc class with the geometry mixins, and adds configuration management through the cget and configure methods, as well as through a partial dictionary interface. The latter can be used to set and query individual options, and is explained in further detail in the next chapter.