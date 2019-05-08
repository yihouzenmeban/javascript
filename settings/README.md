# Git
首先在命令行运行 `git config -l`。

macOS 用户出现的结果里面如果没有 `core.autocrlf=input`，在命令行输入运行 `git config --global core.autocrlf input`。

windows 用户出现的结果里面如果没有 `core.autocrlf=true`，在命令行输入运行 `git config --global core.autocrlf true`。

# Vscode
### 介绍
[ESlint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) 是 VS Code 上用于检查提示代码一致性和格式化 javascript 的插件。

### 安装
直接点击 [ESlint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) 里面的 install 按钮下载安装, 或者用内置插件下载功能搜索下载安装。

第一种: 点击 install 按钮下载安装

![ESlint1.1](https://images.koolearn.com/fe_upload/2019/5/2019-5-8-1557303725667.jpg)

第二种 用内置插件下载功能下载安装
1. 第一步

![ESlint2.1](https://images.koolearn.com/fe_upload/2019/5/2019-5-8-1557303788391.jpg)

2. 第二步

![ESlint2.2](https://images.koolearn.com/fe_upload/2019/5/2019-5-8-1557303832068.jpg)

### 配置

配置文件在 `Code->Preferences->Settings` 下。搜索 eslint, 点击 `Edit in setting.json`, 如下图所示:

![setting](https://images.koolearn.com/fe_upload/2019/5/2019-5-8-1557303902170.jpg)

然后再配置文件中添加以下信息:

```javascript
"eslint.options": {
    "configFile": "/Users/liwei/work/static/grunt-tools/eslint-conf/es6.js" // 注意该配置文件路径为 static 下的 grunt-tools/eslint-conf/es6.js, 需要自行替换成自己电脑中的绝对路径
}
```

ps: 由于目前只支持配置单种配置文件检测, 所以目前只配置了 es6 的检测和格式化, 所以不建议开启保存自动格式化, 建议手动触发格式化。快捷键是 `shift+alt+F`。

![format](https://images.koolearn.com/fe_upload/2019/5/2019-5-8-1557303943843.jpg)

对于从 Sublime 迁移到 VS Code 的用户, 强烈建议使用 [Sublime Text Keymap and Settings Importer](https://marketplace.visualstudio.com/items?itemName=ms-vscode.sublime-keybindings), 在 VS Code 上使用 Sublime 的快捷键设置规则。无痛切换。

# Sublime
### 介绍
[JsFormat](https://github.com/jdc0589/JsFormat) 是 Sublime 上用于格式化 js 的插件。

### 安装
直接使用 Package Control 进行安装。 Windows `Ctrl + Shift + p`, Mac `Command + Shift + p`呼出控制板，如果之前没有安装 Package Control，[点我](https://packagecontrol.io/installation)。

![jsformat_01](https://images.koolearn.com/fe_upload/2019/5/2019-5-8-1557303201388.jpg)

![jsformat_02](https://images.koolearn.com/fe_upload/2019/5/2019-5-8-1557303427474.jpg)

有时候会提示下图，是因为网络原因，可以使用 vpn 再试一次，或者手动安装。

![Screen Shot 2017-03-10 at 16.54.09](https://images.koolearn.com/fe_upload/2019/5/2019-5-8-1557303537820.jpg)

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
    "wrap_width": 120, // 超过 120 个字符串自动折行，ps：用于配合我们的规则，字符串不用换行。
    "rulers": [120], // 编辑器出现 120 个字符串位置的基准线，用作提醒写函数语句时注意换行。
}
```

# Webstorm

尝试了多种导入导出办法，一直没法成功，所以只能麻烦大家自己手动一个个点了。。感谢截图提供者及设计师[阴璐斌同学](https://github.com/yinlubin1989)。

配置修改位置：`Preferences->Editor->Code Style->Javascript`

![未标题-2](https://images.koolearn.com/fe_upload/2019/5/2019-5-8-1557303625000.jpg)

大家碰到有什么格式化出现的问题，联系我哈。。
