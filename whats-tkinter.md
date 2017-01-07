**[上一页](index.md)**    **[下一页](hello-tkinter.md)**

----------

## Tkinter 是什么? ##

Tkinter 模块（“Tk接口”）是来自Scriptics（由 Sun Labs 开发）的 Tk GUI 工具包的标准 Python 接口。

Tk 和 Tkinter 在大多数Unix平台以及Windows和Macintosh系统上都可用。 从8.0版本开始，Tk在所有平台上提供原生外观。

Tkinter由多个模块组成。 Tk接口由名为_tkinter的二进制扩展模块提供。 该模块包含对Tk的低级接口，并且不应该被应用程序员直接使用。 它通常是共享库（或着 DLL ），但在某些情况下可能与Python解释器静态链接。

公共接口通过许多Python模块提供。 最重要的接口模块是Tkinter模块本身。 要使用Tkinter，所需要做的就是导入Tkinter模块：

    import Tkinter

或者，更多的时候这样导入：

    from Tkinter import *

Tkinter 模块仅导出窗口组件类和相关联的常量，因此在大多数情况下可以安全地使用 from-in 形式。 如果你不想这样做，但还是要尽量减少一些输入，你可以使用 import-as 形式：

    import Tkinter as Tk

<br>
----------

**[上一页](index.md)**    **[下一页](hello-tkinter.md)**