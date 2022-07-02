> [https://www.imooc.com/video/19450](https://www.imooc.com/video/19450)

## 1  vim的概念和基本操作
vim是vi发展过来的unix以及类unix系统文本编辑器，功能非常强大，是学习linux必备利器。
### 1.1 打开/创建文件
首先同时打开两个文件，空格隔开即可，没有则创建
```bash
vim file1 file2
```
页面显示的是第一个文件的界面file1,展示和切换命令如下,处于命令模式下：
```bash
使用 :ls 会列举当前缓冲区，然后使用 :b n 跳转到第n个缓冲区
:bpre :bnext :bfirst :blast
或者使用 :b buffer_name 加上tab补全来跳转
```
### 1.2 编辑文件
也就是编辑器的主要功能，编辑文件，最多的场景就是从正常模式进入到插入模式。

| 键盘符 | 说明 |
| --- | --- |
| `i` | insert,是在光标所在的字符之前插入需要录入的文本。 |
| `a` | append,是在光标所在的字符之后插入需要录入的文本。 |
| `o` | open a line below,是光标所在行的**下一行行首**插入需要录入的文本。 |
| `I` | insert before line,是在光标所在行的**行首**插入需要录入的文本。 |
| `A` | append after line,是在光标所在行的行尾插入需要录入的文本。 |
| `O` | append a line above,是光标所在行的**上一行行首**插入需要录入的文本。 |
| `s` | 删除光标所在处的字符然后插入需要录入的文本。 |
| `S` | 删除光标所在行，在当前行的行首开始插入需要录入的文本。 |
| `cw` | 删除从光标处开始到该单词结束的所有字符 |
| `Ctrl+n/p` | 编辑模式下的代码提示功能 |

### 1.3 为什么有那么多模式
vim常用的有四大模式：

- 正常模式 (Normal-mode)：因为大部分情况都是浏览和移动代码，并不需要修改
- 插入模式 (Insert-mode)：开发模式，用来修改配置和开发需求
- 命令模式 (Command-mode)：代替鼠标做一些操作
- 可视模式 (Visual-mode)：块状选择文本，批量操作
### 1.4 插入模式小技巧
#### 1.4.1 如何快速纠错
| 键盘符 | 说明 |
| --- | --- |
| `ctrl+h` | 删除上一个字符 |
| `ctrl+w` | 删除上一个单词 |
| `ctrl+u` | 删除当前行 |

#### 1.4.2 如何快速切换 insert模式和normal模式

- Esc退出插入模式
- Ctrl+c或者Ctrl+[
- gi 快速跳转到你最后一次编辑的地方并进入到插入模式
### 1.5 Vim快速移动大法（normal模式）
#### 1.5.1 反人类的hjkl

- 据说编辑器作者在编写vim的时候键盘还没有流行上下左右按键
- 左（h），j（下），k（上），右（l），移动也不会让手指离开主键盘区
#### 1.5.2 在单词之间飞舞

- w/W移动下一个word/WORD开头，e/E下一个word/WORD尾
- b/B回到上一个word/WORD开头，可以理解为backword
- word指的是以非空白符分割的单词，WORD以空白符分割的单词
#### 1.5.3 行间搜索移动

- f{char}可以移动到char字符上， t移动到char的前一个字符
- 如果第一次没有搜到，可以用 **分号;或者逗号,** 继续搜改行下一个/上一个
- F返回来搜前面的字符
####  1.5.4 Vim水平移动

- 0移动到行首的第一个字符，^移动到第一个非空白字符
- $移动到行尾，g_移动到行尾非空白符
#### 1.5.5 Vim页面移动

- gg/G会定位到文件开头或结尾，可以使用Ctrl+o快速返回
- H/M/L跳转到屏幕的开头，中间和结尾
- Ctrl+u,Ctrl+f，上下翻页，zz定位到屏幕中间
### 1.6 Vim快速增删改查
#### 1.6.1 Vim快速删除

- Vim在normal模式下使用x快速删除一个字符
- 使用d 配合文本对象快速删除一个单词，daw(d around word)
- d和x 都可以搭配数字来执行多次，4x：删除4个字符
- dt{char}，删除到char字符位置
#### 1.6.2 Vim快速修改

- 常用的三个， r（replace），c（change），s（substitute）
- r可以快速替换一个字符，s替换并进入到插入模式
-  c配合文本对象，可以快速修改并进入插入模式
#### 1.6.3 Vim查询

-   使用/或者？进行前向或者反向搜索
- 使用n/N跳转下一个或者上一个匹配
- 使用*或者#进行当前单词的前向或者后向匹配
### 1.7 Vim如何搜索替换（命令模式）

- :[range]s[ubstitute]/{pattern}/{string}/[flags]
- range表示范围，比如：10，20表示10-20行，%表示全部
- pattern表示要替换的模式，string是替换后文本
- flags表示标志位， g(global)表示全局范围内执行，c(confirm)表示确认，可以确认或者拒绝修改
- n(number)报告匹配到的次数而不替换，可以用来查询匹配次数
### 1.8  Vim的 Text Object(文本对象）
**文本对象操作方式**

- [number]<command>[text object]
- number表示次数，command是命令，d(elete),c(hange),y(ank)
- text object是要操作的文本对象，比如单词w,句子s,段落p
### 1.9 Vim复制粘贴
#### 1.9.1 Vim Normal模式复制粘贴

- 复制粘贴分别使用y(yank) 和p(put)，剪切d和p
- 可以使用v(visual)命令选中要复制的地方，然后使用p粘贴
- 配合文本对象使用：yiw复制一个单词，yy复制一行
#### 1.9.2 Vim Insert模式下复制粘贴

- 使用ctrl+c, ctrl+v
- 设置set paste和set nopaste，防止乱序
### 1.10 Vim使用宏完成批量操作
#### 1.10.1 什么是宏？

- 宏可以看作一系列命令的集合
- 可以使用宏录制一系列操作，然后回放到多文本上
#### 1.10.2 如何使用宏

- vim使用q来录制，也是q来结束录制
- q{register}选择要保存的寄存器，把录制的命令保存其中
- @{register}来回放宏到多文本上
#### 1.10.3 如何批量给每行url都加上双引号
```bash
https://www.imooc.com/video/19455i
https://www.imooc.com/video/19455i
https://www.imooc.com/video/19455i
https://www.imooc.com/video/19455i
https://www.imooc.com/video/19455i
https://www.imooc.com/video/19455i
https://www.imooc.com/video/19455i
https://www.imooc.com/video/19455i
https://www.imooc.com/video/19455i
https://www.imooc.com/video/19455i
```

- 授权在normal模式下使用qa，也就是开启宏录制，使用a为寄存器名称
- 然后A移动到第一行行首，插入模式下加上",Esc退出使用A移动到行尾，插入模式加上"
- 然后Esc,按q结束录制，此时已经有一个宏记录了加引号的过程，使用Visual模式，选中下面所有的文本，: normal @a 回车即应用到所选文本，测试成功
### 1.11 Vim补全大法
常用的三种补全方式

- 使用ctrl+n 和ctrl+p补全单词
- 使用ctrl+c 和ctrl+f补全文件名
- 使用ctrl+x 和ctrl+o补全代码，需要开启文件类型检查，安装插件
## 2 Vim配置，我的Vim我做主



