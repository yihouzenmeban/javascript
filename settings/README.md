# Sublime
### 介绍
[JsFormat](https://github.com/jdc0589/JsFormat) 是 Sublime 上用于格式化 js 的插件。

### 安装
直接使用 Package Control 进行安装。 Windows `Ctrl + Shift + p`, Mac `Command + Shift + p`呼出控制板，如果之前没有安装 Package Control，[点我](https://packagecontrol.io/installation)。

![jsformat_02](https://ww3.sinaimg.cn/large/006tKfTcly1fdgum9kf1xj30ej01yt8n.jpg)

![jsformat_02](https://ww3.sinaimg.cn/large/006tKfTcly1fdgum9orn6j30bm0b7t9t.jpg)

### 配置
配置文件在 `Preferences->Package Settings->JsFormat` 下，你可以通过修改 `Settings - User` 去修改配置。
目前咱们项目的配置：

```json
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
    "wrap_line_length": 100,
    "break_chained_methods": true,
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


#Webstorm
下载 [webstorm.jar](webstorm.jar)，在设置里面 `Code Style` 中导入即可。

大家碰到有什么格式化出现的问题，联系我哈。。
