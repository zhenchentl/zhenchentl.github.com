---
layout: post
title: Sublime Text 配置Python, Java, Markdown环境
category: technology
tags:
- Sublime Text
- 开发环境
keywords: Sublime Text,开发环境
description:
---

> 之前用过不少类型的代码/文本编辑器，能够一举解决我所有编辑需求的工具还真是少之又少。曾经坚持用了三个月的Vim，还整理了一份[vimrc在这](https://github.com/zhenchentl/dotvim)。但是Vim学习曲线实在太陡，爬不上去，只好转向这款号称“性感无比”的代码编辑器Sublime Text，又说“程序员必备神器”。一入手才发现，简直是相见恨晚。今天偷空写下我的配置心得，以飨各位看官。

我个人喜欢Sublime Text的原因主要有四：跨Windows/Linux/Mac平台，轻量，安装插件方便，编辑体验极其流畅。平时的编辑需求包括，常规的文本编辑和浏览，用Markdown写文章，写Python和Java代码。接下来我也主要围绕这四类需求来简要介绍我的Sublime Text最基本的配置情况。

Sublime Text 有两个版本，ST3是Beta版，但是至今没觉得有什么影响使用的bug，我用的是[Sublime Text 3 ](http://www.sublimetext.com/3)。

#插件安装
Sublime Text 插件安装很简单，一般有两种方式。

##离线安装
- 下载插件。
- 解压后，放入`Packages`目录中。找到`Packages`目录的简单方法是在Sublime Text 3 的`Preferences`菜单中选择`Browse Packages`。
- 重启 Sublime Text 3.

##在线安装
Sublime Text 有一个Packages的管理插件，[Sublime Package Control](https://sublime.wbond.net/)。通过 Sublime Package Control，安装、升级和卸载 Package 也变得轻松写意了。

- 打开 Sublime Text 3，按下`Control + '`调出 Console。
- 将以下代码粘贴进命令行中并回车:

{% highlight python %}
    import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
{% endhighlight %}

- Sublime Text 2 请使用以下代码：

{% highlight python %}
    import urllib2,os; pf='Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler( ))); open( os.path.join( ipp, pf), 'wb' ).write( urllib2.urlopen( 'http://sublime.wbond.net/' +pf.replace( ' ','%20' )).read()); print( 'Please restart Sublime Text to finish installation')
{% endhighlight %}

- 重启 Sublime Text 3，如果在 `Preferences -> Package Settings`中见到`Package Control`这一项，就说明安装成功了。

通过Package Control 来安装插件：

1, 按下`Shift + Command + P`调出命令面板。
2, 输入`install`调出`Package Control: Install Package`选项，按下回车。
3, 输入插件名称并回车，稍等几秒就安装好了，有的插件可能需要重启Sublime Text才能激活。

#常规配置
每个人的编辑习惯不一样，作为轻微强迫症患者，我喜欢给自己的编辑器做一些设置，例如文件编码，主题，字体，字体大小，显示行号，设置Tab大小，空格替换制表，显示空白符，显示80字符打印线等等。我的初级Settings-User内容如下。

{% highlight %}
{
    "font_size": 12,
    "ignored_packages":
    [
        "Vintage"
    ],
    "font_face": "Consolas",
    // 设置tab的大小为4
    "tab_size": 4,
    // 使用空格代替tab
    "translate_tabs_to_spaces": true,
    // 添加行宽标尺
    "rulers": [80, 100],
    // 显示空白字符
    "draw_white_space": "all",
    // 保存时自动去除行末空白
    "trim_trailing_white_space_on_save": true,
    // 保存时自动增加文件末尾换行
    "ensure_newline_at_eof_on_save": true,
    // 默认编码格式
    "default_encoding": "UTF-8"
}
{% endhighlight %}

下面是一些我配置的常规插件。

- [IMESupport](https://github.com/chikatoike/IMESupport)sublime text 有个BUG，那就是不支持中文的鼠标跟随（和PS类似输入的光标和文字候选框不在一起）,IMESupport可以完美解决这个问题。
- [SideBarEnhancements](https://sublime.wbond.net/packages/SideBarEnhancements)SideBarEnhancements 是一款很实用的右键菜单增强插件，有以 diff 形式式显示未保存的修改、在文件管理器中显示该文件、复制文件路径、在侧边栏中定位该文件等功能，也有基础的诸如新建文件/目录，编辑，打开/运行，显示，在选择中/上级目录/项目中查找，剪切，复制，粘贴，重命名，删除，刷新等常见功能。
- [ConvertToUTF8](https://github.com/seanliang/ConvertToUTF8)通过本插件，您可以编辑并保存目前编码不被 Sublime Text 支持的文件，特别是中日韩用户使用的 GB2312，GBK，BIG5，EUC-KR，EUC-JP 等。ConvertToUTF8 同时支持 Sublime Text 2 和 3。
- [Terminal](https://github.com/wbond/sublime_terminal)这个插件可以让你在Sublime中直接使用终端打开你的项目文件夹，并支持使用快捷键`Control + Shift + T`。
- [Git](https://sublime.wbond.net/packages/Git)这个插件会将Git整合进你的SublimeText，使的你可以在SublimeText中运行Git命令，包括添加，提交文件，查看日志，文件注解以及其它Git功能。
- [GitGutter](https://github.com/jisaacks/GitGutter)在编辑器的凹槽区，依照 Git ，增加小图标来标识一行是否被插入、修改或删除。在 GitGutter 的 [readme](https://github.com/jisaacks/GitGutter)中有说明如何更改颜色图标来更新你的配色方案文件。
- [BracketHighlighter](https://sublime.wbond.net/packages/BracketHighlighter)可以使括号高亮匹配，这个需要自己来配置配色方案。我的配置方案见最后（Bracket settings-User和主题文件Monokai Extended.sublime-package添加的代码）。
- [Monokai Extended](https://github.com/jonschlinkert/sublime-monokai-extended)比较喜欢Soda Dark和Monokai，这里有Monokai Extended。这个 color scheme 是 Monokai Soda 的增强。如果再配合 Markdown Extended，将大大改善 Markdown 的语法高亮。

#配置Markdown书写环境
- [Markdown Extended](https://github.com/jonschlinkert/sublime-markdown-extended)一款Markdown高亮主题，安装后在右下角的语言栏选择Markdown Extended激活这种语言高亮，也可以在`Control + shift + p`启用set syntax:markdown extended
- [Markdown preview](https://github.com/revolunet/sublimetext-markdown-preview)Sublime Text 提供了对Markdown语言的支持，Markdown preview可实现Markdown转换HTML并预览的功能。`Control + B`生成HTML文档，`Alt + m`可直接在浏览器打开。配置快捷键方式如下。

{% highlight python %}
    `{ "keys": ["alt+m"], "command": "markdown_preview", "args": { "target": "browser"} }`
{% endhighlight %}

#配置Python编辑环境
- [SublimeTmpl](https://github.com/kairyou/SublimeTmpl)Sublime Text 新建文件的模板插件。模板支持自定义 `attr`（在settings-user里设置）。
- [SublimeCodeIntel](https://github.com/SublimeCodeIntel/SublimeCodeIntel)为部分语言增强自动完成+成员/方法提示功能，包括了 Python 。这个插件同时也可以让你跳转到符号定义的地方，通过按住 alt 并点击符号。非常方便。支持所有Komodo Editor 支持的语言类型（需要自行配制）`JavaScript, Mason, XBL, XUL, RHTML, SCSS, Python, HTML, Ruby, Python3, XML, Sass, XSLT, Django, HTML5, Perl, CSS, Twig, Less, Smarty, Node.js, Tcl, TemplateToolkit, PHP.`此处仅介绍配置python.
选择`Preferences-->Browser Packages...`进入相关的目录`SublimeCodeIntel\.codeintel`找到config.修改配置文件config。添加：

{% highlight python %}
{
    "Python": {
        "python":'D:/Program Files/Python26/python.exe',
        "pythonExtraPaths": ['D:\Python34','D:\Python34\DLLs','D:\Python34\Lib','D:\Python34\Lib\site-packages','D:\Python34\libs']
    }
}
{% endhighlight %}

#配置Java编辑环境

###前提
JDK已经安装好，Java环境已配置好。

###配置编译环境
- 在%Sublime Text 安装目录`%/package/`中找到`Java.sublime-package`,用好压或者其他压缩软件打开(重命名为`rar`文件双击就可以打开)，在里面找到`JavaC.sublime-build`并打开。里面添加：

{% highlight python %}
{
    "shell_cmd": "runJava.bat \"$file\"",
    "file_regex": "^(...*?):([0-9]*):?([0-9]*)",
    "selector": "source.java",
    //添加下面一段可支持编译中文，亲测Java可用
    "encoding": "GBK"
}
{% endhighlight %}

- 在java的`JDK/bin`路径下，新建一个文件，命名为`runJava.bat`，里面内容为：

{% highlight python %}
    @ECHO OFF
    cd %~dp1
    IF EXIST %~n1.class (
    DEL %~n1.class
    )
    javac -encoding UTF-8 %~nx1
    IF EXIST %~n1.class (
    java %~n1
    )
{% endhighlight %}

- 写一个Hello world 测试代码，使用`Control + B`运行之。

{% highlight java %}
    public class test{
        public static void main(String[] args) {
            System.out.println("Hello world!");
        }
    }
{% endhighlight %}

#附录

###BracketHighlighter配置

Bracket settings-User

{% highlight python %}
    {
        "bracket_styles": {
            // This particular style is used to highlight
            // unmatched bracket pairs. It is a special
            // style.
            "unmatched": {
                "icon": "question",
                "color": "brackethighlighter.unmatched",
                "style": "highlight"
            },
            // User defined region styles
            "curly": {
                "icon": "curly_bracket",
                "color": "brackethighlighter.curly",
                "style": "highlight"
            },
            "round": {
                "icon": "round_bracket",
                "color": "brackethighlighter.round",
                "style": "outline"
            },
            "square": {
                "icon": "square_bracket",
                "color": "brackethighlighter.square",
                "style": "outline"
            },
            "angle": {
                "icon": "angle_bracket",
                "color": "brackethighlighter.angle",
                "style": "outline"
            },
            "tag": {
                "icon": "tag",
                "color": "brackethighlighter.tag",
                "style": "outline"
            },
            "single_quote": {
                "icon": "single_quote",
                "color": "brackethighlighter.quote",
                "style": "outline"
            },
            "double_quote": {
                "icon": "double_quote",
                "color": "brackethighlighter.quote",
                "style": "outline"
            },
            "regex": {
                "icon": "regex",
                "color": "brackethighlighter.quote",
                "style": "outline"
            }
        }
    }
{% endhighlight %}

Monokai Extended.sublime-package添加的代码

{% highlight python  %}
    <!-- BEGIN Bracket Highlighter plugin color modifications -->
    <dict>
        <key>name</key>
        <string>Bracket Default</string>
        <key>scope</key>
        <string>brackethighlighter.default</string>
        <key>settings</key>
        <dict>
            <key>foreground</key>
            <string>#FFFFFF</string>
            <key>background</key>
            <string>#A6E22E</string>
        </dict>
    </dict>

    <dict>
        <key>name</key>
        <string>Bracket Unmatched</string>
        <key>scope</key>
        <string>brackethighlighter.unmatched</string>
        <key>settings</key>
        <dict>
            <key>foreground</key>
            <string>#FFFFFF</string>
            <key>background</key>
            <string>#FF0000</string>
        </dict>
    </dict>

    <dict>
        <key>name</key>
        <string>Bracket Curly</string>
        <key>scope</key>
        <string>brackethighlighter.curly</string>
        <key>settings</key>
        <dict>
            <key>foreground</key>
            <string>#FF00FF</string>
        </dict>
    </dict>

    <dict>
        <key>name</key>
        <string>Bracket Round</string>
        <key>scope</key>
        <string>brackethighlighter.round</string>
        <key>settings</key>
        <dict>
            <key>foreground</key>
            <string>#E7FF04</string>
        </dict>
    </dict>

    <dict>
        <key>name</key>
        <string>Bracket Square</string>
        <key>scope</key>
        <string>brackethighlighter.square</string>
        <key>settings</key>
        <dict>
            <key>foreground</key>
            <string>#FE4800</string>
        </dict>
    </dict>

    <dict>
        <key>name</key>
        <string>Bracket Angle</string>
        <key>scope</key>
        <string>brackethighlighter.angle</string>
        <key>settings</key>
        <dict>
            <key>foreground</key>
            <string>#02F78E</string>
        </dict>
    </dict>

    <dict>
        <key>name</key>
        <string>Bracket Tag</string>
        <key>scope</key>
        <string>brackethighlighter.tag</string>
        <key>settings</key>
        <dict>
            <key>foreground</key>
            <string>#FFFFFF</string>
            <key>background</key>
            <string>#0080FF</string>
        </dict>
    </dict>

    <dict>
        <key>name</key>
        <string>Bracket Quote</string>
        <key>scope</key>
        <string>brackethighlighter.quote</string>
        <key>settings</key>
        <dict>
            <key>foreground</key>
            <string>#56FF00</string>
        </dict>
    </dict>
    <!-- END Bracket Highlighter plugin color modifications -->
{% endhighlight %}
