# microsoft office 的 COM/VSTO 插件

起因是我在电脑里面下载了金山WPS用来看一些公司的文件，直接用 MS 的 WPS 打开的时候会有一些奇怪的无法显示的字符
结果删除了金山WPS之后发现， MS 的 WPS 里面居然还有 金山 WPS 的功能
遂简单AI查询了一下

## 疑虑

我观察了一下，发现 MS wps 里面不只有 金山PDF 的功能栏， 还有我之前下载的 福昕PDF 的功能栏
所以我想，是不是 MS wps 本身就有开发类似于插件式的接口，供其他应用可以进行嵌入呢？
查询后发现果然如此

右键功能区的自定义当中可以看出这部分英文的项目和其他内部功能格格不入，只需要关掉就可以看起来比较干净一些，甚至连 office plus 也可以通过这种方式关闭
不知这个 office plus 的生态位到底是什么，总是添加一些神秘的 ai 功能，但是又需要开启会员，并不直接作为 ms wps 的功能发布，说不定是另一个项目组做的内容呢
这个 COM 插件是需要装载的，所以在电脑内还有对应的残留，也需要下次来进行仔细的检查

但由于时间原因，还有其他事情需要处理，这个文章姑且留待下次整理

## 和 GPT 的问答内容

### 问

office 的word excel ppt是有预留插件接口吗？
我发现我原有的相对纯净的word excel ppt莫名出现了office plus的功能，这也许是office本家的还不算奇怪。但我因为工作需要下载了金山wps（有一些格式需要），此时word excel ppt中的工具栏目中出现了wps pdf的标签页，还有一些新的英文templete，ai功能在office原有的“开始”工具栏目中，是金山wps带来的。
于是我继续观察，发现了我之前下载的福昕PDF也有任务栏标签页嵌入在word excel ppt中。
这种行为是什么？控制反转？接口嵌入？比较正式的称呼是什么？
即使我卸载了wps，这些插入的按钮依然存在，甚至点击后还会自动下载金山wps的installer。

### 答

我先把这类现象按 Office 的扩展机制、第三方软件的安装行为、以及卸载残留三层拆开解释；你遇到的不是单纯“界面皮肤”，更像是 Office 加载项/COM 插件被注册进了宿主程序。

Word / Excel / PowerPoint 本身就有预留扩展接口。你看到的这种现象，比较正式的说法通常是：
Office 加载项 / Office Add-in / COM Add-in / VSTO Add-in / Ribbon 扩展
你描述的 WPS PDF、福昕 PDF、Office Plus 这些标签页，本质上大概率是第三方软件在安装时把自己的 Office 插件注册进了 Word、Excel、PowerPoint，让它们在 Office 的功能区 Ribbon 里显示按钮或新标签页。

#### 为什么 PDF 软件、WPS 会进入 Word / Excel / PPT？

因为它们希望在 Office 文件里提供这些功能：

比如 PDF 软件常见功能是：

在 Word 里一键转 PDF；
在 Excel 里导出 PDF；
在 PPT 里生成 PDF；
加水印、书签、安全设置；
合并、压缩、转换格式。

福昕官方就有 “Foxit PDF Creator Add-in for Microsoft Office” 这类插件，用于从 Office 里生成 PDF，并提供安全、书签、水印等额外控制。

WPS 也类似。它除了自身是 Office 套件，还会安装 PDF、模板、AI、格式转换等入口。安装器可能会把这些能力注册到 Microsoft Office 的功能区中。

#### 为什么卸载 WPS 后按钮还存在？

这一般不是正常的“完全卸载”，而是以下几种情况之一：

1. 卸载器没有清理 Office 插件注册项
WPS 主程序卸载了，但它注册到 Office 的加载项信息还留在注册表或 Office 配置里。于是 Word 仍然认为：
我这里有一个 WPS PDF 插件。
但真正的程序文件可能已经没了，点击后就会报错、无响应，或者触发重新安装。

2. 留下了独立的 WPS PDF / WPS Office Assistant / 模板组件
很多套件不是一个程序，而是一组组件。你卸载了 WPS Office，不一定卸载了：
WPS PDF；
WPS Office Add-in；
WPS AI Assistant；
WPS Template；
Kingsoft Office 相关组件；
更新器 / 安装器 / 服务。

#### 你可以怎么清理？

先从 Office 内部处理：

在 Word / Excel / PowerPoint 里分别检查：
文件 → 选项 → 加载项
底部有一个“管理”下拉框，依次看：

COM 加载项；
Word 加载项 / Excel 加载项 / PowerPoint 加载项；
禁用项目。

然后点“转到”，取消勾选或删除可疑项，比如：

WPS PDF；
Kingsoft；
Foxit；
Office Plus；
Template；
AI Assistant。

微软官方也提供了在 Word、Excel、PowerPoint 中查看、管理、移除加载项的入口说明。
如果 Office 里禁用不了，再去 Windows：
设置 → 应用 → 已安装的应用
搜索这些关键词：

WPS
Kingsoft
金山
Foxit
福昕
PDF Creator
Office Plus
Template
Add-in

把残留组件卸载。

如果还存在，就可能需要清注册表。这个阶段要谨慎，关键词通常在这些位置附近：

HKEY_CURRENT_USER\Software\Microsoft\Office\Word\Addins
HKEY_CURRENT_USER\Software\Microsoft\Office\Excel\Addins
HKEY_CURRENT_USER\Software\Microsoft\Office\PowerPoint\Addins

HKEY_LOCAL_MACHINE\Software\Microsoft\Office\Word\Addins
HKEY_LOCAL_MACHINE\Software\Microsoft\Office\Excel\Addins
HKEY_LOCAL_MACHINE\Software\Microsoft\Office\PowerPoint\Addins

64 位系统还可能在：

HKEY_LOCAL_MACHINE\Software\WOW6432Node\Microsoft\Office

不熟悉注册表的话，不建议直接删。更安全的做法是先在 Office 加载项面板里禁用，或者使用 Autoruns 这类工具查看 Office Addins 项。
