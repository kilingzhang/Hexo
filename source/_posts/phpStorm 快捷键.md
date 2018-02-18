---
title: phpStorm 快捷键
tags:
  - phpStorm
  - PHP
id: 143
categories:
  - PHP
  - 软件工具
date: 2017-02-04 17:26:22
---

# phpStorm

## 快捷键：

*   ctrl+shift+alt+n: 快速打开指定的method，field.弹出一个对话框，输入标识符，选择后跳转到指定内容
*   alt+F7:           查找选定变量，方法被调用的地方。选中一个方法或者变量，查找出所有调用的地方
*   ctrl+F12:         弹出一个对话框，显示当前文件里的所有方法，变量，直接输入方法变量名，回车即可跳转到定义位置
*   alt+F1:           快速打开当前编辑文件在其他tool windows里，这个很好用的键盘
*   ese:              退出tool windows，焦点返回到编辑器里
*   shift+esc:        退出并隐藏tool windows,焦点返回编辑器里
*   F12:              光标从编辑器里移动到最后一个关闭的tool windows里
<!--more-->
*   ctrl+alt+d:       快速复制多行，哈哈，这个vim里更加简单
*   ctrl+shift+p:     显示函数方法的参数列表
*   ctrl+shift+backspace: last edit location
*   ctrl+shift+F7:    在当前文件高亮选定的标识符，esc退出高亮，f3,shift f3向下向上导航高亮标识符
*   ctrl+shift+alt+e: exploer最近打开的文件
*   alt+方向键：      左右在打开的编辑器标签间切换，上下在打开的文件中的方法里上下切换
*   alt+shift+c:      浏览最近的修改历史
*   ctrl+`:           选择主题，不常用
*   ctrl+tab:         switcher,在已打开文件之间或者工具窗口间切换
*   alt+alt:          连续两次快速按下alt键不放，显示tool windows（project,database ...)
*   ctrl+k:           快速调用 commit changes 对话框
*   alt+F3:           显示搜索窗格，对当前文件进行搜索，然后配合ctrl+alt+r,可以进行替换操作
*   ctrl+shift+f:     find in path 在指定文件夹或者整个project内搜索，ctrl+shift+r进行替换操作
*   ctrl+shift+alt+t: 快速rename，里面有好几个选项，慢慢理解吧
*   shift+F6:         rename，自动重命名该变量所有被调用的地方
*   ctrl+shift+n:     快速导航到指定文件，弹出一个dialog，输入文件名即可
*   ctrl+shift+i:     快速查找选中内容定义的位置 quick definition viewer
*   ctrl+n:           快速打开任意类，弹出一个对话框，输入类名称，跳转到类文件

## 常用快捷键(keymaps:Default情况下)

*   Esc键编辑器（从工具窗口）
*   F1   帮助 千万别按,很卡!
*   F2（* Shift+F2）  下/上高亮错误或警告快速定位
*   F3   向下查找关键字出现位置
*   F4   查找变量来源
*   F5   复制文件/文件夹
*   F6   移动
*   F11  切换书签
*   F12  返回到以前的工具窗口

`注意：部分快捷键，必须在没有更改快捷键的情况下才可以使用`

## 查询快捷键

*   Ctrl+N   查找类
*   Ctrl+* Shift+N  查找文件，打开工程中的文件(类似于eclipse中的* Ctrl+* Shift+R)，目的是打开当前工程下任意目录的文件
*   Ctrl+* Shift+* Alt+N 查 找类中的方法或变量(JS)
*   CIRL+B   找变量的来源，跳到变量申明处
*   Ctrl+* Alt+B  找所有的子类
*   Ctrl+* Shift+B  找变量的 类
*   Ctrl+G   定位行，跳转行
*   Ctrl+F   在当前窗口查找文本
*   Ctrl+* Shift+F  在指定路径查找文本
*   Ctrl+R   当前窗口替换文本
*   Ctrl+* Shift+R  在指定路径替换文本
*   Alt+* Shift+C  查找修改的文件，最近变更历史
*   Ctrl+E   最近打开的文件
*   F3   查找下一个
*   Shift+F3  查找上一个
*   F4   查找变量来源
*   Ctrl+* Alt+F7  选 中的字符 查找工程出现的地方
*   Alt+F7 直接查询选中的字符
*   Ctrl+F7  文件中查询选中字符

## 自动代码

*   Alt+回车  导入包,自动修正
*   Ctrl+* Alt+L  格式化代码
*   Ctrl+* Alt+I  自动缩进
*   Ctrl+* Alt+O  优化导入的类和包
*   Ctrl+E  最近更改的文件/代码
*   Ctrl+* Shift+SPACE 切换窗口
*   Ctrl+SPACE空格  代码自动完成，代码提示,一般与输入法冲突
*   Ctrl+* Alt+SPACE  类 名或接口名提示（与系统冲突）
*   Ctrl+P   方法参数提示，显示默认参数
*   Ctrl+J   自动代码提示，自动补全
*   Ctrl+* Alt+T  把选中的代码放在 TRY{} IF{} ELSE{} 里
*   Alt+INSERT  生成代码(如GET,SET方法,构造函数等)

## 复制快捷方式

*   F5   复制文件/文件夹
*   Ctrl+C   复制
*   Ctrl+V   粘贴
*   Ctrl+X   剪 切,删除行
*   Ctrl+D   复制行
*   Ctrl + Y    删除行插入符号
*   Ctrl+* Shift+V  可以复制多个文本

## 高亮

*   Ctrl+F   选中的文字,高亮显示 上下跳到下一个或者上一个
*   F2（* Shift+F2） 高亮错误或警告快速定位
*   Shift+F2  高亮错误或警告快速定位
*   Ctrl+* Shift+F7  高亮显示多个关键字.

## 本地历史VCS/SVN

*   Alt +反引号（'） 快速弹出VCS菜单
*   Ctrl + K         提交项目VCS
*   Ctrl + T         更新项目从VCS
*   Alt + * Shift + C  查看最近发生的变化

## 其他快捷方式

*   Ctrl+Z        倒退(代码后悔)
*   Ctrl+* Shift+Z  向前
*   Ctrl+H        显 示类结构图
*   Ctrl +F12      文件结构弹出
*   Ctrl+* Shift+H  方法的层次结构
*   Ctrl+* Alt+H    呼叫层次
*   Ctrl+Q   显示代码注释
*   Ctrl+W   选中代码，连续按会 有其他效果
*   Ctrl+* Shift+W   减少当前选择到以前的状态
*   Ctrl+B   转到声明，快速打开光标处的类或方法说明注释(* Ctrl + 鼠标单击 也可以)
*   Ctrl+O   魔术方法
*   Ctrl+/   注释//取消注释*   Ctrl+* Shift+/  注释/_..._/
*   Ctrl+ []   光标移动到 {}[]开头或结尾位置
*   Ctrl+* Shift+[]    选中块代码，可以快速复制
*   Ctrl + '-/+': 可以折叠项目中的任何代码块,包括htm中的任意nodetype=3的元素，function,或对象直接量等等。它不是选中折叠，而是自动识别折叠。

*   Ctrl + '.': 折叠选中的代码的代码

*   Ctrl+* Shift+U   选中的字符大小写转换

*   Ctrl+* Shift+i      快速查看变量或方法定义源
*   Ctrl+* Alt+F12  资源管理器打开文件夹，跳转至当前文件在磁盘上的位置
*   Alt+F1   选择当前文件或菜单中的任何视图工具栏
*   Shift+* Alt+INSERT 竖编辑模式

*   Ctrl+* Alt ←/→  返回上次编辑的位置

*   Alt+ ←/→  切换代码视图，标签切换
*   Alt+ ↑/↓  在方法间快速移动定位
*   Alt + '7': 显示当前的类/函数结构。类似于eclipse中的outline的效果。试验了一下，要比aptana的给力一些，但还是不能完全显示prototype下面的方法名。
*   Shift+F6  重命名,重构 当前区域内变量重命名/重构不但可以重命名文件名，而且可以命名函数名，函数名可以搜索引用的文件，还可以重命名局部变量。还可以重命名标签名。在sublime text中有个类似的快捷键：* Ctrl+* Shift+d。

*   Ctrl+* Shift+enter(智能完善代码 如 if())

*   Ctrl+* Shift+up/down(移动行、合并选中行，代码选中区域 向上/下移动)*   Ctrl+UP/DOWN  光标跳转到编辑器显示区第一行或最后一行下
*   ESC   光标返回编辑框
*   Shift+ESC  光 标返回编辑框,关闭无用的窗口
*   Ctrl+F4   关闭当前的编辑器或选项卡

*   Ctrl + * Alt + V引入变量

*   Ctrl + * Alt + F 类似引入变量
*   Ctrl + * Alt + C引入常量

*   Ctrl + Tab   键切换选项卡和工具窗口

*   Ctrl + * Shift + A  查找快捷键
*   Alt + ＃[0-9]      打开相应的工具窗口
*   Ctrl + * Shift + F12 切换最大化编辑器
*   Alt + * Shift + F    添加到收藏夹
*   Alt + * Shift + I    检查当前文件与当前的配置文件
*   Ctrl +反引号（`）  快速切换目前的配色/代码方案/快捷键方案/界面方案
*   Ctrl + * Alt + S     打开设置对话框（与QQ冲突）

## 运行

*   Alt + * Shift + F10  选择的配置和运行
*   Alt + * Shift + F9   选择配置和调试
*   Shift + F10        运行
*   Shift + F9调试
*   Ctrl + * Shift + F10运行范围内配置编辑器
*   Ctrl + * Shift + X运行命令行

## 调试

*   F8步过
*   F7步入
*   Shift + F7智能进入
*   Shift + F8步骤
*   Alt + F9运行到光标
*   Alt + F8计算表达式
F9恢复程序
*   Ctrl + F8切换断点
*   Ctrl + * Shift + F8查看断点

## 导航

*   Shift + Esc键隐藏活动或最后一个激活的窗口
*   Ctrl + * Shift + F4关闭活动运行/消息/ / ...选项卡
*   Ctrl + * Shift + Backspace键导航到最后编辑的位置
*   Ctrl + * Alt+B   到实施（S）
*   Ctrl + * Shift+I  打开快速定义查询
*   Ctrl + U        转到super-method/super-class
*   Alt + Home      组合显示导航栏

## 书签

*   Ctrl + F11切换书签助记符
*   Ctrl +＃[0-9]转到编号书签
*   Shift + F11显示书签

## 编辑

*   Ctrl + Q      快速文档查询
*   Alt + INSERT  生成的代码...器（getter，setter方法，构造函数）
*   Ctrl + O      覆盖方法
*   Ctrl + I      实现方法

*   Alt + Enter   显示意图的行动和快速修复

*   Shift + Tab   键缩进/取消缩进选中的行

*   Ctrl + * Shift + J  智能线连接（仅适用于HTML和JavaScript）

*   Ctrl + Enter      智能线分割（HTML和JavaScript）
*   Shift + Enter     开始新的生产线

*   Ctrl + Delete   删除字（word）

*   Ctrl + Backspace删除字开始
*   Ctrl +小键盘+ / - 展开/折叠代码块
*   Ctrl + * Shift +小键盘+展开全部
*   Ctrl + * Shift +数字键盘关闭全部