# Git
首先在命令行运行 `git config -l`。

macOS 用户出现的结果里面如果没有 `core.autocrlf=input`，在命令行输入运行 `git config --global core.autocrlf input`。

windows 用户出现的结果里面如果没有 `core.autocrlf=true`，在命令行输入运行 `git config --global core.autocrlf true`。

# Sublime
### 介绍
[JsFormat](https://github.com/jdc0589/JsFormat) 是 Sublime 上用于格式化 js 的插件。

### 安装
直接使用 Package Control 进行安装。 Windows `Ctrl + Shift + p`, Mac `Command + Shift + p`呼出控制板，如果之前没有安装 Package Control，[点我](https://packagecontrol.io/installation)。

![jsformat_01](https://ww3.sinaimg.cn/large/006tKfTcly1fdh3pe3vwxj30g20b5aao.jpg)

![jsformat_02](https://ww1.sinaimg.cn/large/006tKfTcly1fdh3pgocfkj30fm09t0th.jpg)

有时候会提示下图，是因为网络原因，可以使用 vpn 再试一次，或者手动安装。

![Screen Shot 2017-03-10 at 16.54.09](https://ww2.sinaimg.cn/large/006tKfTcly1fdhuybxvuhj30bx05x0t3.jpg)

### 配置

配置文件在 `Preferences->Package Settings->JsFormat` 下，你可以通过修改 `Settings - User` 去修改配置。
目前咱们项目的配置：

```javascript
{
    "indent_size": 4,
    "indent_char": " ",
    "indent_with_tabs": false,
    "indent_level": 0,
    "space_in_paren": false,
    "space_in_empty_paren": false,
    "preserve_newlines": true,
    "max_preserve_newlines": 3,
    "brace_style": "collapse-preserve-inline",
    "space_after_anon_function": false,
    "comma_first": false,
    "keep_array_indentation": false,
    "keep_function_indentation": false,
    "wrap_line_length": 120,
    "break_chained_methods": false,
    "unescape_strings": false,
    "end_with_newline": true,

    "format_on_save": false,
    "jsbeautifyrc_files": false,
    "ignore_sublime_settings": false,
    "format_on_save_extensions": ["es6", "js", "json"]
}
```

`format_on_save` 可以自行设置是否保存的时候自动进行格式化整理，别的选项是目前咱们固定的，请不要修改。
我们也可以使用快捷键进行格式化整理，默认快捷键 Windows 使用 `ctrl+alt+f`, Mac 使用 `control+alt+f`。

另外，建议大家在 Sublime 的配置文件中加上以下配置信息：

```javascript
{
    "tab_size": 4,  // tab 为四个空格
    "translate_tabs_to_spaces": true, // tab 转为空格
    "trim_trailing_white_space_on_save": true, // 保存时自动去除行末多余空格
    "ensure_newline_at_eof_on_save": true, // 保存时文件末尾自动添加空行
    "default_line_ending": "unix", // 使用 unix 的 LF 换行符进行换行
    "word_wrap": true, // 自动折行
    "wrap_width": 120, // 超过 100 个字符串自动折行，ps：用于配合我们的规则，字符串不用换行。
    "rulers": [120], // 编辑器出现 100 个字符串位置的基准线，用作提醒写函数语句时注意换行。
}
```

# Webstorm

尝试了多种导入导出办法，一直没法成功，所以只能麻烦大家自己手动一个个点了。。感谢截图提供者及设计师[阴璐斌同学](https://github.com/yinlubin1989)。

配置修改位置：`Preferences->Editor->Code Style->Javascript`

![未标题-2](https://ww3.sinaimg.cn/large/006tKfTcly1fdhkm20reyj30ux0x9dmo.jpg)

大家碰到有什么格式化出现的问题，联系我哈。。
