## 默认快捷键
1. Editor basics 基础
	+ Ctrl + Shift + A : 搜索可用的作用
	+ Ctrl + Shift + 向右箭头 : 光标选中到该字符结束位置
	+ Ctrl + W : 选中当前代码块，可用重复按会选择更多
	+ Ctrl + Shift + W : 缩小选中代码块，与上相反
	+ Ctrl + / ： 注释/取消注释行
	+ Ctrl + D : 复制行
	+ Ctrl + Y : 删除行
	+ Ctrl + Shift + 向上/向下箭头 : 方法位置上下换
	+ Ctrl + 加/减键(-/+) : 展开/折叠光标方法
	+ Ctrl + Shift + 加/减键(-/+) : 展开/折叠文件方法
	+ Ctrl + Alt + T : 选中代码模板
	+ Ctrl + Shift + Delete : 去掉代码模板
 	+ Alter + Shift + 向上/向下箭头 : 代码行上下换
 	+ Alter + J : 选中相同的字符
 	+ Alter + Shift + J : 与上同情况使用，则返回上一次结果
 	+ Ctrl + Alter + Shift + J : 选中所有相同的字符
2.  Code Completion 代码补全/完成
	+ Ctrl + 空格 : 声明基本完成，列出选择匹配建议; 选中用tab覆盖后一单词
	+ Ctrl + Shift + Enter : 完成声明
	+ Ctrl + Shift + 空格 : ①上下文，列出选择匹配建议；②按两次用于return提示
	+ Alter + Enter : 列出选择匹配建议
	+ **.** : 在后缀是使用，列出可选匹配
3. Refactorings 重构
	+ Ctrl + Alt + V : 提取变量/字段，则声明一个
	+ Ctrl + Alt + M : 提示一些行封装成一个方法调用
	+ Ctrl + Alt + C : 声明为成常数
	+ Ctrl + Alt + P : 提取到方法的参数中
	+ Shift + F6 : 重写完按Enter完成
4. Code Assistance 代码协助
	+ Ctrl + Alt + L : 代码格式化
	+ Ctrl + P : 查看方法参数
	+ Ctrl + Q : 查看方法文档，按Esc退出
	+ Ctrl + Shift + I : 查看方法的实现
	+ Ctrl + F1 : 查看错误描述
	+ Ctrl + Shift + F7 : 高亮该符号
	+ F2 : 跳到下一个错误的地方
5. Navigation 导航
	+ F4 : 跳到资源源头
	+ Ctrl + B : 跳到声明的类或者接口
	+ Ctrl + Alt + B : 查看类或者接口的实现
	+ Ctrl + F12 : 查看文件结构
	+ Ctrl + F : 代码查找，按F3向下找；按Shift+F3向上找/返回上一个找到；按Esc退出；

## 其它
* 文件重命名: Shift + F6
* 搜索jdk中的类: ctrl + n
* 查看接口的实现类: ctrl + alt + b
* 查看类或接口的继承关系： ctrl + h
* 查看类的继承关系图形: ctrl + alt + shift + u
* 大小写转换: ctrl + shift + u
* 查找与替换: 当前文件 ctrl + r | 整个项目 ctrl + shit + r 
* 复制类的限定名: ctrl + shift + alt + c
* 撤销与恢复: ctrl + z | ctrl + shift + z
* 跳转到父类: ctrl + U
* 查看类的方法: alt + 7

## debug
* F7 步入：进入到方法内部执行。一般步入自定义的方法。区别于强行步入
* F8 步过：不会进入到方法内部，直接执行
* F9恢复程序：下面有断点则运行到下一断点，否则结束程序
* Shift+F7 智能步入：断点所在行上有多个方法调用，会弹出进入哪个方法进行选择
* Alt+Shift+F7 强行步入：能进入任何方法，包括官方类库等第三方方法　
* Shift+F8 步出：跳出步入的方法
* Ctr+F8 设置断点，已有断点则是去掉断点
* Alt+F8 计算表达式。选中对象，弹出可输入计算表达式调试框，查看该输入内容的调试结果
* Alt+F9 运行到光标处，不需要断点
* Alt+F10 在任何页面时都可以跳转到当前代码执行的行

### idea学习，不过里面教学版本较旧，不过影响不大：[IntelliJ-IDEA-Tutorial](https://github.com/judasn/IntelliJ-IDEA-Tutorial)