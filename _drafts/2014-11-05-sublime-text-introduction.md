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

我个人喜欢Sublime Text的原因主要有四：跨Windows/Linux/Mac平台，轻量，安装插件方便，编辑体验极其流畅。平时的编辑需求包括，常规的文本编辑和浏览，用Markdown写文章，写Python和Java代码。接下来我也主要围绕这四类需求来简要介绍我的Sublime Text配置情况。

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
    import urllib.request,os; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); open(os.path.join(ipp, pf), 'wb').write(urllib.request.urlopen( 'http://sublime.wbond.net/' + pf.replace(' ','%20')).read())
- Sublime Text 2 请使用以下代码：
    import urllib2,os; pf='Package Control.sublime-package'; ipp = sublime.installed_packages_path(); os.makedirs( ipp ) if not os.path.exists(ipp) else None; urllib2.install_opener( urllib2.build_opener( urllib2.ProxyHandler( ))); open( os.path.join( ipp, pf), 'wb' ).write( urllib2.urlopen( 'http://sublime.wbond.net/' +pf.replace( ' ','%20' )).read()); print( 'Please restart Sublime Text to finish installation')
- 重启 Sublime Text 3，如果在 `Preferences -> Package Settings`中见到`Package Control`这一项，就说明安装成功了。

通过Package Control 来安装插件：

1, 按下`Shift + Command + P`调出命令面板。
2, 输入`install`调出`Package Control: Install Package`选项，按下回车。
3, 输入插件名称并回车，稍等几秒就安装好了，有的插件可能需要重启Sublime Text才能激活。

#常规配置
每个人的编辑习惯不一样，作为轻微强迫症患者，我喜欢给自己的编辑器做一些设置，例如文件编码，主题，字体，字体大小，显示行号，设置Tab大小，空格替换制表，显示空白符，显示80字符打印线等等。我的初级Settings-User内容如下。
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
        "default_encoding": "UTF-8",
    }
下面是一些我配置的常规插件。
- [IMESupport](https://github.com/chikatoike/IMESupport)sublime text 有个BUG，那就是不支持中文的鼠标跟随（和PS类似输入的光标和文字候选框不在一起）,IMESupport可以完美解决这个问题。
- [SideBarEnhancements](https://sublime.wbond.net/packages/SideBarEnhancements)SideBarEnhancements 是一款很实用的右键菜单增强插件，有以 diff 形式式显示未保存的修改、在文件管理器中显示该文件、复制文件路径、在侧边栏中定位该文件等功能，也有基础的诸如新建文件/目录，编辑，打开/运行，显示，在选择中/上级目录/项目中查找，剪切，复制，粘贴，重命名，删除，刷新等常见功能。


#配置Markdown书写环境

如果你喜欢 Soda Dark 和 Monokai，我建议你使用 Monokai Extended (GitHub)。这个 color scheme 是 Monokai Soda 的增强，如果再配合 Markdown Extended (GitHub)，将大大改善 Markdown 的语法高亮。
#配置Python编辑环境

#配置Java编辑环境
