**[上一页](widget-configuration.md)**    **[下一页]( )**

----------

# 控件样式 #

所有Tkinter标准部件都提供了一组基本的“样式”选项，允许您修改每个部件的颜色、字体和其他视觉特性。


## 颜色 ##

大多数组件允许您使用背景和前景选项指定组件和文本颜色。要指定颜色，可以使用颜色名称，也可以显式指定红色(**R**ed)、绿色(**G**reen)和蓝色(**B**lue)（即RGB）颜色组件。

### 颜色名称 ###

Tkinter包括一个颜色数据库，它将颜色名称映射到相应的RGB值。这个数据库包括一些常见的名字，如*Red*、 *Green*、 *Blue*、 *Yellow*和*LightBlue*，但也有一些更具异国情调的东西，如Moccasin、PeachPuff等。

在X窗口系统上，颜色名称由X服务器定义。您可能能够找到名为xrgb.txt文件，它包含颜色名称和相应的RGB值的列表。在Windows和Macintosh系统上，颜色名称表内置在Tk中。

在Windows下，还可以使用Windows系统颜色（用户可以通过控制面板更改这些颜色）：


    SystemActiveBorder,  
    SystemActiveCaption,   
    SystemAppWorkspace,   
    SystemBackground,   
    SystemButtonFace,   
    SystemButtonHighlight,   
    SystemButtonShadow,   
    SystemButtonText,   
    SystemCaptionText,   
    SystemDisabledText,   
    SystemHighlight,   
    SystemHighlightText,   
    SystemInactiveBorder,   
    SystemInactiveCaption,   
    SystemInactiveCaptionText,   
    SystemMenu,   
    SystemMenuText,   
    SystemScrollbar,   
    SystemWindow,   
    SystemWindowFrame,   
    SystemWindowText.

在Macintosh上，可以使用以下系统颜色：

    SystemButtonFace,   
    SystemButtonFrame,   
    SystemButtonText,    
    SystemHighlight,    
    SystemHighlightText,    
    SystemMenu,    
    SystemMenuActive,    
    SystemMenuActiveText,    
    SystemMenuDisabled,    
    SystemMenuText,    
    SystemWindowBody.

颜色名称不区分大小写。许多（但不是全部）颜色名称也可以在单词之间加空格或不带空格。例如，““lightblue””、“light blue”和“Light Blue”都指定相同的颜色。

### RGB规范 ###

如果需要显式指定颜色，可以使用以下格式的字符串：

    #RRGGBB

RR、GG、BB分别是红色、绿色和蓝色值的十六进制表示。以下示例显示如何将颜色3元组转换为Tk颜色规范：

``` python
    tk_rgb = "#%02x%02x%02x" % (128, 192, 200)
```

Tk还支持“#RGB”和“#RRRRGGGGBBBB”形式来分别描述16和65536个级别的颜色值。

可以使用组件的 *winfo_rgb* 方法将颜色字符串（名称或RGB形式）转换为3元组：

``` python
rgb = widget.winfo_rgb("red")
red, green, blue = rgb[0]/256, rgb[1]/256, rgb[2]/256
``` 
请注意，winfo_rgb返回16位rgb值，范围从0到65535。要将它们映射到更常见的0-255范围内，必须将每个值除以256（或向右移动8位）。  

## 字体 ##

显示文本的组件允许您允许您指定使用哪种字体。所有的组件都提供合理的默认值，您很少需要为标签和按钮等简单的元素指定字体。

字体通常使用 *font* 组件选项指定。Tkinter支持多种不同的字体描述符类型：

### 字体描述符 ###

- 用户定义的字体名称
- 系统字体
- X字体描述符
对于8.0之前的Tk版本，只支持X字体描述符（见下文）。

字体描述符 #  
从tk8.0开始，Tkinter支持独立于平台的字体描述符。可以将字体指定为元组，该元组包含族名称、高度（以点为单位）以及可选的具有一个或多个样式的字符串。示例：

``` python
("Times", 10, "bold")
("Helvetica", 10, "bold italic")
("Symbol", 8)
```
要获得默认大小和样式，可以将字体名称作为单个字符串。如果字体族名称不包含空格，也可以向字符串本身添加大小和样式：  
``` python
"Times 10 bold"
"Helvetica 10 bold italic"
"Symbol 8"
```
以下是大多数Windows平台上可用的一些字体族：  

Arial (corresponds to Helvetica), Courier New (Courier), Comic Sans MS, Fixedsys, MS Sans Serif, MS Serif, Symbol, System, Times New Roman (Times), and Verdana:

![](widget-styling-01.gif)

请注意，如果字体族名称包含空格，则必须使用上述元组语法。  

可用的样式有normal(常规)、bold(粗体)、roman(罗马)、italic(斜体)、underline(下划线)和overstrike(删除线)。

TK8.0自动将Courier、Helvetica和Times映射到所有平台上相应的本地字体族。此外，在tk8.0下，字体规范永远不会失败；如果Tk不能找到一个完全匹配的字体，它会尝试找到一个类似的字体。如果失败，Tk将返回到特定于平台的默认字体。Tk关于什么是“足够相似”的想法可能与您自己的观点不符，因此您不应该过分依赖这个特性。

Windows下的tk4.2也支持这种字体描述符。有几个限制，包括字体族名必须存在于平台上，并且并非所有上述样式名都存在（或者说，其中一些具有不同的名称）。


### 字体名称 ###
此外，Tk8.0允许您创建命名字体，并在为组件指定字体时使用它们的名称。  

tkFont模块提供了一个字体类，允许您创建字体实例。在Tkinter接受字体说明符的任何地方都可以使用这样的实例。还可以使用字体实例获取字体度量，包括用该字体编写的给定字符串所占的大小。

``` python 
tkFont.Font(family="Times", size=10, weight=tkFont.BOLD)
tkFont.Font(family="Helvetica", size=10, weight=tkFont.BOLD,
            slant=tkFont.ITALIC)
tkFont.Font(family="Symbol", size=8)
```
如果修改命名字体（使用config方法），更改将自动传播到使用该字体的所有组件。

**Font**构造函数支持以下样式选项（请注意，常量是在 **tkFont**模块中定义的）：

**family**  字体族

**size**  以磅为单位的字体大小。要以像素为单位指定大小，请使用负值。

**weight**  字体粗细。使用**NORMAL**或**BOLD**。默认值为**NORMAL**。

**slant**  字体倾斜。使用**NORMAL**或**ITALIC**。默认值为**NORMAL**。

**underline**  字体下划线。如果为1（真），字体加下划线。默认值为0（false）。

**overstrike**  字体删除线。如果为1（真），则在使用此字体编写的文本上绘制一条线。默认值为0（false）。

### 系统字体 ###
Tk还支持特定于系统的字体名称。在X System下，这些通常是字体别名，如 **fixed** 、**6x10**等。

在Windows下，包括 **ansi**、**ansifixed**、**device**、**oemfixed**、**system**和**systemfixed**：

![](widget-styling-02.gif)

在Macintosh上，系统字体名是 **application** 和 **system** 。

注意，系统字体是完整的字体名称，而不是族名，并且它们不能与大小或样式属性组合。出于可移植性的原因，尽可能避免使用这些名称。

### X字体描述符 ###
X字体描述符是具有以下格式的字符串（星号表示通常不相关的字段）。有关详细信息，请参阅Tk文档或X手册）：

    -*-family-weight-slant-*--*-size-*-*-*-*-charset

字体系列通常是像 **Times** 、**Helvetica**、**Courier**或**Symbol**之类的。

*weight* 不是 **Bold** 就是 **Normal**。斜体可以是R代表“罗马”（normal），I代表斜体，或者O代表斜体（实际上，这只是斜体的另一个词）。

*size* 是字体的高度，以分点为单位（即点数乘以10）。通常每英寸有72个点，但一些低分辨率显示器可能使用更大的“逻辑”点来确保小字体仍然清晰易读。最后，字符集通常是 **ISO8859-1**（ISO Latin 1），但对于某些字体可能有其他值。

以下描述符要求使用ISO Latin 1字符集的12点黑体乘以字体：

    -*-Times-Bold-R-*--*-120-*-*-*-*-ISO8859-1

如果您不关心字符集，或使用具有特殊字符集的类似字体的符号，则可以使用单个星号作为最后一个组件：

    -*-Symbol-*-*-*--*-80-*

一个典型的X服务器至少支持 **Times**、**Helvetica**、**Courier**和其他一些字体，大小如8、10、12、14、18和24点，以及普通、粗体和斜体（**Times**）或斜体（**Helvetica**，**Courier**）变体。大多数服务器也支持可自由缩放的字体。您可以使用**xlsfonts**和**xfontsel**等程序来检查在给定服务器上您可以访问哪些字体。

这种字体描述符也可以在Windows和Macintosh上使用。请注意，如果您使用tk4.2，您应该记住字体系列必须是Windows支持的字体系列（见上文）。

## 文本格式 ##

虽然文本标签和按钮通常包含一行文本，但Tkinter也支持多行。若要跨行拆分文本，只需在必要的地方插入换行符（**\n**）。

默认情况下居中的。可以通过将 **justify** 选项设置为**LEFT**或**RIGHT**来更改此设置。默认值为**CENTER**。

您还可以使用 **wraplegth** 选项设置最大宽度，并让组件自己将文本包装在多行上。Tkinter试图用空格包装，但如果组件太窄，它可能会跨行打断单个单词。

## 边框 ##

所有组件都有边框（虽然有些窗口组件的边框并不都是可见的）。边界由可选的3D浮雕和焦点高亮区域组成。

### 浮雕 ###

浮雕设置控制如何绘制控件边框：

- **borderwidth** (**bd**)  
这是边框的宽度，以像素为单位。大多数组件的默认边界宽度为一到两个像素。几乎没有任何理由使边界更宽。

- **relief**  
此选项控制如何绘制三维边界。它可以设置为 **SUNKEN**, **RAISED**, **GROOVE**, **RIDGE**, **FLAT**.

### 焦点亮点 ###

高亮设置控制如何指示组件（或其子组件）具有键盘焦点。在大多数情况下，突出显示区域是浮雕外部的边界。以下选项控制如何绘制此额外边界：

- **highlightcolor**  
此选项用于在组件具有键盘焦点时绘制突出显示区域。它通常是黑色的，或者其他一些明显的对比色。

- **highlightbackground**  
此选项用于在组件没有焦点时绘制突出显示区域。它通常与widget背景相同。

- **highlightthickness**  
此选项是高光区域的宽度，以像素为单位。它通常是一个或两个像素的组件，可以获取键盘焦点。

## 光标 ##
- **cursor**  
此选项控制将鼠标移到组件上时要使用的鼠标光标。

如果未设置此选项，组件将使用与其父对象相同的鼠标指针。

请注意，有些组件（包括文本和输入框组件）默认设置此选项。

![](widget-styling-03.gif)

----------

**[上一页](widget-configuration.md)**    **[下一页]( )**
