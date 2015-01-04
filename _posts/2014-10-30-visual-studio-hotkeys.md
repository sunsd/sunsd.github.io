---
layout: post
title: VS 快捷键
category: Windows
tags: hotkeys
---
###常用快捷键
注：部分项需用Ctrl+K代替Ctrl+E。


 组合键   | 说明 
:---------|:---------
| ###代码快捷键 
ctrl+w | 选中一个单词
Ctrl + M + O | 折叠所有方法
Ctrl + M + M | 折叠或者展开当前方法
Ctrl + M + L | 展开所有方法
Shift+Alt+Enter | 全屏
Ctrl+E+C/Crtr+E+U | 注释选定内容
`Ctrl+K+F` | 代码格式化
Ctrl+Shift+空格键 / Ctrl+K,P | 参数信息
Ctrl+K,I | 快速信息
Ctrl+K,M | 生成方法存根
Ctrl+K,X | 插入代码段
Ctrl+K,S | 插入外侧代码
F12 | 转到所调用过程或变量的定义
| ###窗口快捷键 
Ctrl+W,W | 浏览器窗口
Ctrl+W,S | 解决方案管理器
Ctrl+W,C | 类视图
Ctrl+W,E | 错误列表
Ctrl+W,O | 输出视图
trl+W,P | 属性窗口
Ctrl+W,T | 任务列表
Ctrl+W,X | 工具箱
Ctrl+W,B | 书签窗口
Ctrl+W,U | 文档大纲
Ctrl+D,B | 断点窗口
Ctrl+D,I | 即时窗口
Ctrl+Tab | 活动窗体切换
Ctrl+Shift+N | 新建项目
Ctrl+Shift+O | 打开项目
Ctrl+Shift+S | 全部保存
Shift+Alt+C | 新建类
Ctrl+Shift+A | 新建项
Shift+Alt+Enter | 切换全屏编辑
Ctrl+F2 | 切换书签开关
F2 | 移动到下一书签
Ctrl+B,P | 移动到上一书签
Ctrl+B,C | 清除全部标签
Ctrl+I | 渐进式搜索
Ctrl+Shift+I | 反向渐进式搜索
Ctrl+F | 查找
Ctrl+Shift+F | 在文件中查找
F3 | 查找下一个
Shift+F3 | 查找上一个
Ctrl+H | 替换
Ctrl+Shift+H | 在文件中替换
Alt+F12 | 查找符号(列出所有查找结果)
Ctrl+Shift+V | 剪贴板循环
Ctrl+左右箭头键 | 一次可以移动一个单词
Ctrl+上下箭头键 | 滚动代码屏幕，但不移动光标位置。
`Ctrl+Shift+L` | 删除当前行
Ctrl+M,M | 隐藏或展开当前嵌套的折叠状态
Ctrl+M,L | 将所有过程设置为相同的隐藏或展开状态
Ctrl+M,P | 停止大纲显示
Ctrl+E,S | 查看空白
Ctrl+E,W | 自动换行
Ctrl+G | 转到指定行
Shift+Alt+箭头键 | 选择矩形文本
`Alt+鼠标左按钮` | 选择矩形文本
Ctrl+Shift+U | 全部变为大写
Ctrl+U | 全部变为小写

###一些小技巧
- 如果你想复制一行代码，只需要在这行的空白处 CTRL+C。 同理，删除或者剪贴一行CTRL+X。 如果想复制一段在{}的代码，直接在头或者尾 CTRL+C.

- 显示方法里的参数 CTRL+SHIFT+space.

- Tab+Tab,可以调出代码段

- 渐进式搜索自动搜索整个文档或窗口中的所有文本，但是已折叠或隐藏的文本除外。

- 在项目中有些代码没有完成，我们可以做一下标记，便于将来查找。

```
创建方法：在要标志的地方输入：//TODO:...内容...
使用方法：视图—>任务列表—>注释
```

- 命令行快速启动

```
"sqlwb"  快速启动SQL2005企业管理器
"devenv" 启动相应版本的VS
```

- **F12、Ctrl+减号、CTRL + SHIFT + 减号**
这三个键在查看代码的时候，特别有用。通过F12你可以快速的找到一个函数的定义，通过Ctrl+减号你可以快速的返回到函数的调用处。

- 输入类名后在类名上按 Ctrl+. 
即可自动添加该类的引用命名空间语句。

- 怎样创建区域以方便代码的阅读？

```
#region
代码区域
#endregion
```
- 怎样调用智能提示？
两种方法：Ⅰ. Ctrl+J Ⅱ. Alt+→

- 怎样快速切换不同的窗口？
Ctrl+Tab


###调试

- 调试.全部中断 CTRL + BREAK 临时停止执行调试会话中的所有进程。仅适用于“运行”模式。
- 调试.断点 CTRL + B 显示“断点”对话框，在此可添加和修改断点。
- 调试.调用堆栈 CTRL + ALT + C 显示“调用堆栈”窗口，以显示当前执行线程的所有活动过程或堆栈帧列表。仅适用于“运行”模式。
- 调试.清除所有断点 CTRL + SHIFT + F9 清除项目中的所有断点。
- 调试.启用断点 CTRL + F9 在当前行上设置断点。
- 调试.异常 CTRL + SHIFT + E 显示“异常”对话框。
- 调试.即时 CTRL + ALT + I 显示“即时”窗口，在该窗口中可以计算表达式并执行单个的命令。
- 调试.局部变量 CTRL + ALT + L 显示“局部变量”窗口，VS2008快捷键以查看当前堆栈帧中每个过程的变量及其值。调试.进程 CTRL + SHIFT + R 显示“进程”对话框，该对话框允许在单个解决方案中同时调试多个程序。
- 调试.快速监视 SHIFT + F9 显示带有选定表达式的当前值的“快速监视”对话框。仅适用于“中断”模式。使用该命令可检查尚未为其定义监视表达式的变量、属性或其他表达式的当前值。
- 调试.重新启动 CTRL + SHIFT + F5 终止调试会话，重新生成，然后从开始处开始运行应用程序。可用于“中断”模式和“运行”模式。

