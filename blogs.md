
## git

* `HEAD`：表示当前commit
* `HEAD^`：上一个commit是
* `HEAD^^`：上上一个commit
* `HEAD~100`：前第100个的commit

### config

命令行设置

```sh
git config http.proxy  # 查看
git config --list  # 查看所有
git config --global http.proxy http://127.0.0.1:7890  # 设置
```

或者编辑`~/.gitconfig`

```
[core]
	editor = vim
[user]
	name = 
	email = 
[alias]
    l = log --all --decorate --oneline --graph
    ll = log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(white)<%ad>%Creset' --abbrev-commit
    st = status
    cim = commit -m
    amend = commit --amend
    br = branch
    co = checkout
    c = commit
[http]
	proxy = http://127.0.0.1:7890
[https]
	proxy = http://127.0.0.1:7890
```

### tags

```sh
git tag -l  # 列出tag

git push --atomic origin master v1.5  # push with tag
git push origin v1.5  # push单个tag
git push origin --tags  # push全部tag
```

### diff

```
@@ -37,3 +37,4 	@@

\-         +Enjoy it!
```

* `-`表示变动前的版本
* `37`表示第37行
* `3`表示连续3行
* `\-`表示删去

### git branch, git checkout

```sh
git branch newBranch  ##新建

# -m 重命名
# -a 全部列出
# -d 删除
```

```sh
git checkout -b newBranch  # 新建分支
git checkout -m oldName newName  # 重命名
```

### git remote

```sh
git remote add repo url  # 添加远程库
git remote rm repo  # 删除远程库
```


### git rebase

在你已经提交了若干commit，而远程仓库有别人提交了若干你还没pull的commit时，你在push前要先rebase到远程最新版本。

### git merge

merge和rebase的区别在于，会在历史上留下分支记录。

### git stach

将未add的本地修改临时隐藏并存储

```sh
git stash  ## stash
git stash pop  # 恢复
git stash list  # 列出所有的stash
git stash drop  # 删除某个stash
```

### git cherry-pick

```sh
# 将某个commit的diff加到当前分支
git cherry-pick commitID
```

### git show

查看commit的详情

```sh
git show <commit_id>
```

### git rm/clean

```sh
git rm --cached <file>  # 移除追踪，不删除
git rm <file>  # 删除文件并git add

git clean -f  # 删除untracked files
```

### git reset

```sh
# 首先将本地的commit恢复到<commit_id>
git reset <commit_id> <file>  # 将<commit_id>以后的改动放到work区
git reset --soft <commit_id> <file> # 将<commit_id>以后的改动放到add区
git reset --hard <commit_id>  # 将<commit_id>以后的改动移除
```

### submodule

#### 新建

1.  添加已经存在的远端submodule

    ```sh
    git submodule add <URL> <localdir>
    ```

2.  新建submodule

    ```sh
    # 在远程新建文件夹
    git submodule add <URL> <localdir>
    ```

3.  将现有的子目录转为submodule
    
    ```sh
    cd <subdir>
    git init
    git remote add origin <url>
    git push 
    # 和远程同步
    
    # 停止在总目录中对此目录的追踪
    git rm -r  <subidr>
    git cim 
    
    # 添加submodule
    git submodule add <url> <subdir>
    ```

#### 使用

克隆一个带submodule的目录

```sh
git clone --recursive [URL]
# 首次加载submodule
git submodule update --init

# To pull everything including submodule
git pull --recurse-submodules
```

Update submodule

```sh
git submodule udpate
```

#### 删除

```sh
rm -rf <submodule>
# 删除项目目录下.gitmodules文件中子模块相关条目
vim .gitmodules
# 删除配置项中子模块相关条目
vi .git/config
# 删除模块下的子模块目录，每个子模块对应一个目录，注意只删除对应的子模块目录即可
rm .git/module/<submodule>
```

### 显示多git目录下改变的文件

```sh
find . -maxdepth 1 -mindepth 1 -type d -exec sh -c "(cd {} ; if [ -d .git ] ; then echo '**********************' ;  echo {} ; git status --porcelain ; echo; fi)" \;
```

### 显示多git目录下和远程仓库的同步情况

```sh
find . -maxdepth 1 -mindepth 1 -type d -exec sh -c "(cd {} ; if [ -d .git ] ; then echo '**********************' ;  echo {} ; git --no-pager log --decorate=short --pretty=oneline -n1 ; echo; fi)" \;
```

## Sublime

> 如果一个快捷键在mac上用到cmd，在win上用到control，则统称为cc。

* 打开链接：选中链接，右键在浏览器中打开
* 移动行：mac `cmd + control + arrow`, win `shift + control + arrow`
* 删除行：`shift + control + k`
* 在选中的行尾插入：`control + shift + l`
* 多重选中：`cc + d`
* 选中所有：mac `control + cmd + g`, win `alt + f3`
* 隐藏边栏：`cc+k, cc+b`
* 显示函数列表或Markdown目录：`cc + r`


###  Settings


```json
{
  // 不显示
	"file_exclude_patterns": ["*.pyc", "node_modules", "package-lock.json"],

  // 不搜索
  "folder_exclude_patterns": ["node_modules"],
  "binary_file_patterns": ["*.jpg", "*.jpeg", "*.png", "*.gif", "*.ttf", "*.tga", "*.dds", "*.ico", "*.eot", "*.pdf", "*.swf", "*.jar", "*.zip"],

  "save_on_focus_lost": true,
}
```


###  插件

* Open in default application
* A File Icon
* ConvertToUTF8
* Terminus
    ```
	{
	  "keys": ["ctrl+`"],
	  "command": "toggle_terminus_panel"
	}
    ```

### 分屏幕

* 分成1列/2列/3列/4列/网格：`cmd+alt+1/2/3/4/5` 
* 焦点移动到第1/2/3/4列：`control+1/2/3/4` 
* 将当前文件移动到第1/2/3/4列：`shift+control+1/2/3/4` 

### 在侧栏显示当前文件

需要在Keybinding里定义

```
{ "keys": ["ctrl+r"], "command": "reveal_in_side_bar" }
```

## vscode

* 打开偏好设置  `f1 / cc + shift + p`
* 自动补全 `tab` 
* toggle word wrap 是否自动换行 `alt + z`
* 添加/减少缩进 `cmd + []`
* 重命名元素 `f2`


### 行操作

* 删除行 `cc + shift + k`
* 下方插入行 `cc + enter`
* 上方插入行 `cc + shift + enter`
* 选中当前行 `cc + l`
* 上下移动行 `alt + arrow`
* 上下复制行 `shift + alt + arrow`  
* 上下行添加箭头 `cmd + alt + arrow`
* 在选中行的末尾插入光标 `shift + alt + i`

### 多重选择  

* 选择当前词下一次出现的位置 `cc + d` 
* 跳过下一个词 `cc + k`
* 撤销上一次选择 `cc + u`
* 全部选中当前词 `cc + f2`

### 搜索

* 搜索string `cc + f`
* 替换string (mac)`cmd + alt + f` (win)`control + h`
* 全局搜索string (mac)`cmd + shift + f`
* 选中下一个 `enter`
* 选中上一个 `shift + enter`
* 全部选中 `alt + enter` 

### 跳转

* 跳转到定义/回去 `f12/f11` 
* 跳转到定义还可以用 `cc + click`或者`cc + hover`
* 小窗查看定义 `alt + f12`
* 分屏查看定义 `cc + k, f12`
* 跳转到当前分屏中的文件 (win)`ctrl + tab`
* 跳转到文件 `cc + p`
* 跳转到当前文件的第几行: `cc + p`然后`:行数`
* 跳转到当前文件的symbol: `cc + p`然后`@symbol`
* 跳转到匹配括号：`cmd + shift + \`
* 显示当前文件的大纲 (win)`control + shift + >`

### 分屏和侧边栏

* 在同一个分屏中的标签页中切换 (win)`ctrl + tab`  
* 分屏 `alt + \`
* 焦点在不同的分屏间切换 `cmd + 123`, `alt + 左右箭头`
* 焦点切换到explorer `cmd + 0`
* 打开explorer中选中的文件 `cmd + 下箭头`
* 在标签页间切换 `cmd + alt + 左右箭头`
* collapse outline `cmd + 左箭头`
* 关闭除当前分屏 `cmd+k w`

侧栏：
* 隐藏侧栏: `cc+shift+b`
* Explorer: `cc+shift+e`    
* Find: `cc+shift+f`    
* Extensions: `cc+shift+x`    
* Debug: `cc+shift+d`    


### cmd + k

* 打开快捷键面板 `cmd + k, cmd + s`
* compare with saved `cmd + k , d`
* 复制当前文件的路径 `cmd + k , p`
* 进入/退出专注模式 `cmd + k , z`
* select language mode `cmd + k , m`

### 代码块折叠 fold & unfold

* 折叠当前部分: `cmd+alt+[ `           
* 展开当前部分: `cmd+alt+]`            
* 按不同程度折叠: `cmd+k cmd+0,1,2,3,4`     
* 全部展开: `cmd+k cmd+j`             

### Debug  

* F5          start/continue
* shift+F5    stop
* F11         step into
* F10         step over
* shift+F11   step out
* F9          添加breakpoint 

### 插件

* gitlens: 团队开发的时候, 看一行代码是谁写的
* path intellisense : 自动补全路径名称
* css peak: 查看html element的class对应的css

> [插件开发](https://liiked.github.io/VS-Code-Extension-Doc-ZH/#/)

### python配置

venv环境:
1.  进入项目文件夹
2.  点击status bar左下角的Python interpreter
3.  选择venv

## jetbrain

自动补全用tag

###  快捷键

* call lightbulb : `option + enter`
* 提示`.`后面可以跟的内容 在点号后面按`control + space`
* 编辑多行 : `option, option+上下箭头`; 或者`cmd + shift + 8`进入多选模式, 直接拉选框
* 查看文档 : `F1`
* Find action : `shift + cmd + a`  输入line number
* Find : `shift, shift`
* git commit : `cmd + k`


#### 纯文本编辑

* 移动行 `shift + option + arrow`
* 删除行 `cmd + delete`
* 复制行 `cmd + d`
* 跳转到某行 `cmd + l`
* 选择当前词下一次出现的位置 `control + G`
* 替换 `cmd + r` replace
* 搜索类 `cmd + o`
* 搜索文件 `shift + cmd + o`


#### 代码编辑

* 重命名 `shift + f6`
* 重构 `control + t`
* 查看定义 `cmd + hover`
* 跳转到定义 `cmd + click`
* 跳转+回去 `cmd + [/]`
* 快速查看文档 `F1`
* 实现interface `control + i`
* 根据struct创建interface `菜单栏 - refacetor - extract - interface`
* 跳转到interface的implement. 或者可以点击上方的小灯泡 `cmd + option + b`
* 提示括号里可以输入的内容 `shift + control + space`
* 提示可以接受`.`前面的值或者变量为参数的函数: `control + space`两次
* 运行测试 `shift + control + R`

#### 代码折叠

* `cmd -`         fold
* `cmd +`         unfold
* `shift + cmd -`   fold all
* `shift + cmd +`   unfold all


#### 窗口

* 焦点切换 `option + tab`
* 双击tab        收起侧栏
* option + F12     打开terminal 
* cmd + 1          project栏
* cmd + 2          editor窗口
* cmd + ?          根据侧栏上的数字打开相应窗口
* command + E               查看最近文件
* control + tab             在当前打开的标签页中切换

#### 没怎么用到

* shift + command + delete	回到上一次做修改的地方				
* shift + cmd + e             recent locations & recent changed locations
* option + F1				查看当前元素在项目中的位置 
* option + F7				找到当前类在整个项目中出现的所有位置
* command + f12				查看当前文件结构

###  配置

-   Appearance & Behavior -> System Settings
    -   \[] Reopen last project on startup  重启应用时不打开最后一次打开的项目
    -   \[] Confirm application exit
    -   [x] Open project in new window

-   设置终端为zsh
    -   tools - terminal - shell path = /bin/zsh

-   remove file header: 有时候jetbrain会给文件自动加上版权声明的文件头, 如果要去掉文件头
    `Preferences -> Editor -> File and Code Templates -> Includes -> File Header`


-   intelliJ有时候没法启动, 需要手动删除缓存.
    ```
    ~/Library/Caches/<PRODUCT><VERSION>
    ```

-   设置取消注释自动缩进
    `Setting - Code Style - Java - Code Generation - Comment code` 
    取消勾选`Line Comment at first column`, `Block comment at first column`

-   proxy: 翻墙

#### hide files

Preferences | File Types | Ignore files and folders

#### plugins

-   Markdown
-   `command+7`	查看目录结构
-   Markdown Navigate  	提供LaTeX数学公式的预览
-   shifter 			检测光标选中的字,行, 用键盘上的上下箭头移动
-   git toolbox
-   安装dash插件dash查文档 `cmd+shift+d`

#### 颜色

-   Apperance
    -   dark theme
    -   dark window header

-   Preference-Editor-Color Scheme-General:  
    将`Unknown symbol`设置为Background red, Foreground write.

###  删除IDEA配置

除了删除.app文件以外，还需要用命令行删除如下目录下的App名字目录即可，如Goland 2018.2版本，会存在个Goland2018.2的目录

~/Library/Preferences/
~/Library/Caches/
~/Library/Application Support/
~/Library/Logs/

### goland 

#### 快捷键

goland支持postfix completion。例如要输入sort.Strings(message), 可以输入message.sort, 常见的有sort, append, len

* 将表达式包装为变量 variable：`cmd+option+v`
* 去除变量改为表达式：`cmd+option+n`
* 将代码包装成新的函数 method：`cmd+option+m`
* 实现没有实现的类方法：`F2` -> `option+回车` -> `Implement missing methods` 
* 添加format里的变量：`option+enter` -> `add format string argument`

#### 配置

在保存时auto format go文件:
preference -> tools -> file watcher -> 添加gofmt, scope -> current file, 不auto save format, 不然还没写完就会auto format, 会报错


## vim

### Keymap

vim 1.txt 2.txt 默认进入1.txt文件的编辑界面

在查找末尾中加入\c表示大小写不敏感


| 快捷键                                               | 功能              |
| ---------------------------------------------------- | ----------------- |
| 向下翻半页                                           | control + d       |
| 重复上个的命令                                       | .                 |
| 反转字母大小写                                       | ~                 |
| 查找光标所在单词, 要求单词前后为空白字符或者标点符号 | *                 |
| 直接指定打开的文件                                   | :b 2.txt (be)     |
| 当前目录下创建新文件                                 | :e test.txt       |
| 显示当前文件名                                       | :f                |
| 重命名当前文件                                       | :f new.txt        |
| 查看历史命令                                         | q: (回车选择执行) |
| 列出当前打开的文件                                   | :ls               |

#### 命令

| 命令                                               | 功能              |
| ---------------------------------------------------- | ----------------- |
:set nu         |显示行号
:set nonumber  | 隐藏行号
tab |补全命令, 如`:NERDTree`只需要输入`:N`
:q              |退出
:q!             |不保存强制退出
:wq             |保存并退出
:w [path]      | 另存为
:f             | 显示正在编辑的文件名
:f new.txt      |改变正在编辑的文件名字为new.txt
:n         | 编辑下一个文件（2.txt） 
:N         | 编辑上一个（1.txt），可以加 ! 即 :N! 强制切换，之前文件内的输入没有保存，仅仅是切换到另一个文件
:b 2.txt   | 直接进入文件2.txt编辑
:e 3.txt   | 打开新文件3.txt, 这是原本的文件就自动关闭，如果没保存的话，要用:e! 3.txt
:e#         |打开上次的文件
:ls        | 列出当前打开的文件
:s/old/new/g    |    把这一行的字符串old替换为new
:%s/old/new/g   |    把所有的字符串old替换为字符串new
:ter |打开一个终端窗口, exit退出

### 查找

* `/`       查找
* `n`       继续查找
* `N`       反向查找
* 在查找末尾中加入`\c`表示大小写不敏感
* 在查找末尾中加入`\C`表示大小写敏感

#### Insert 插入  

* `i`   在当前光标处编辑
* `a`   在光标后插入编辑
* `I`   在行首插入
* `A`   在行尾插入
* `o`   在行后插入一个新行
* `O`   在行前插入一个新行

#### Jump 跳转  

* `w`   到下一个单词的开头
* `2w`  跳转两个单词
* `e`   到当前单词的结尾, end
* `b`   到前一个单词的开头, back
* `^`   行头
* `$`   行尾
* `nG`  跳转到第n行
* `G`   跳转到最后一行
* `gg`  跳转到第一行

#### 撤销 

* `u`       撤销一次
* `2u`      撤销两次
* `U`       撤销当前行的所有修改 
* `Ctrl+r`  redo，即撤销undo的操作

#### 替换

* `r`       将游标所在字母替换为指定字母 
* `R`       连续替换，直到按下Esc
* `c`       替换选中部分，并进入插入模式
* `cw`      替换一个单词，即删除一个单词，并进入插入模式
* `c2w`     从当前单词开始替换两个单词，并进入插入模式
* `c^`      替换至行首
* `c$`      替换至行尾
* `cc`      替换整行，即删除游标所在行，并进入插入模式  

#### 复制粘贴

* `y`       用shift-v模式选中, 复制
* `yw`      复制一个单词。
* `y2w`     复制两个单词。
* `y^`      复制至行首
* `y$`      复制至行尾
* `yy`      复制游标所在的整行
* `3yy`     从光标处开始复制3行
* `yG`      复制至文档末
* `y1G`     复制至文档开头
* `p`   代表粘贴至光标后
* `P`   代表粘贴至光标前

#### 删除/剪切

* `x`       删除游标所在的字符            
* `X`       删除游标所在前一个字符 
* `dw`      删除一个单词（不适用中文）    
* `d^`      删除至行首                    
* `d$`      删除至行尾                    
* `dd`      删除整行,2dd表示一次删除2行 
* `dG`      删除到文档结尾处              
* `d1G`     删至文档首部      

Q:  删除第30行至第40行?
A:  30,40d

#### Indention  

* `>>`      整行将向右缩进
* `<<`      整行向左回退

#### 分屏 

前面都要加`control+w`

* `s`       将当前窗口分割成两个水平的窗口
* `v`       将当前窗口分割成两个垂直的窗口
* `q`       关闭分割出来的视窗
* `o`       打开一个视窗并且隐藏之前的所有视窗
* `J`       光标移至下面
* `K`       光标移至上面
* `H`       光标移至左边
* `L`       光标移至右边
* `+`       增加视窗的高度



#### 选择模式

* `v`           进入/退出选择模式
* `Shift+v`     行选择模式, 可以上下移动光标选更多的行
* `Ctrl+v`      矩形区域选择

> [Multi-selection](http://vim.wikia.com/wiki/Search_and_replace)  
> [replace](https://vim.fandom.com/wiki/Search_and_replace)
> [vim查找](https://harttle.land/2016/08/08/vim-search-in-file.html)

### 配置

~/.vimrc

* `set nu!` 一直显示行号
* `set ignorecase` 大小写不敏感查找
* `set smartcase` 如果有一个大写字母, 则切换到大小写敏感查找

#### 颜色

> https://github.com/sickill/vim-monokai

Put monokai.vim file in your `~/.vim/colors/` directory and add the following line to your `~/.vimrc`:

```bash
syntax enable
colorscheme monokai
```

查看自带配色方案

```bash
ls /usr/share/vim/vim74/colors
```

在`.vimrc`中添加

```bash
colorscheme morning
```

### 插件

使用vundle管理vim的插件, 插件会下载到`~/.vim/bundle`.

下载vundle:

```bash
git clone https://github.com/gmarik/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

安装插件时, 打开vim输入: 
```
:PluginInstall somePlugin
```

停止使用插件时, 打开`.vimrc`注释掉原来那句plugin: 
```
# Plugin 'Valloric/YouCompleteMe'
```
然后
```
:PluginUpdate
:PluginClean
```

```
:PluginInstall
```
之后按`:q`退出

####  Nerdtree  

:NERDTree   打开Nerdtree

:q          退出Nerdtree

```
control+w+w  在nerdtree的窗口和编辑窗口中切换
回车    进入目录, 打开文件
o       打开关闭文件或目录，如果想打开文件，必须光标移动到文件名
t       在标签页中打开
s和i    水平或纵向分割窗口打开文件
p       到上层目录
C       将当前目录设为根目录
u       将当前目录的上层目录设为根节点
cd      将根目录设为当前路径, 可以在当前路径下新建文件
```

Q:  How to use NERDTree to create new file or directory?
A:  First, bring up NERDTree and navigate to the directory where you want to create the new file. Press `m` to bring up the NERDTree Filesystem Menu. This menu allows you to create, rename, and delete files and directories. Type `a` to add a child node and then simply enter the filename. You’re done! To create a directory follow the same steps but append a / to the filename.

### referecne

- [vim学习资源](https://github.com/vim-china/hello-vim)
- [vim从入门到精通](https://github.com/wsdjeg/vim-galore-zh_cn)

## xcode

### storyboard

- `cmd+shift+L`打开UIKit的Library
- 在`main.storyboard`下, `control+drag`某元素到`ViewController.swift`上, 添加IBAction
- Q:  怎么把模拟器从竖的变成横的?  
  A:  选中模拟器, cmd+方向键
- 插入图片(image literal): 在等号后面直接输入图片的名字
- 添加颜色(color literal): 输入color, 选择Color literal, 双击小方块就可以选择颜色
- Hide status bar:   
  `Project Settings -> Deployment Info -> Status Bar Style -> Hide status bar`
- Size to Fit Content for fonts: `cmd=`

### 设置

- `preference -> Navigation -> Uses Focused Editor`
  默认是Use Primary Editor, 会导致总是在primary editor里打开文件.

- `preference -> behavior -> running-starts -> show debugger with variable and console view`

### 显示

左边的面板 
```
cmd+0           显示和隐藏左边的面板
cmd+1~7         显示左侧面板各个tab
```

右边的面板 
```
cmd+option+0    显示和隐藏右边的面板
cmd+option+1~6  显示右侧面板各个tab
```

```
cmd+shift+Y         显示和隐藏下面的面板
cmd+option+enter    打开assistant editor
cmd+enter           关闭assistant editor
```

```
option+放大缩小     固定中心放大缩小storyboard的UI
cmd+=               确保字体全部显示
option+click        并排打开文件
cmd+control+w       关闭并排打开的文件
```

### Shortcuts
```
cmd+R           运行
cmd+B           编译
```

行操作
```
control+K       删除整行
cmd+[]          缩进
cmd+option+[    上移
cmd+option+]    下移
cmd+L           跳转到指定行
```

查找与替换
```
cmd+F           当前文件内搜索
shift+cmd+F     全局搜索
cmd+G           搜索下一处
```

文件
```
cmd+shift+o     打开指定文件    
cmd+shift+J     快速定位当前文件在项目中的位置
选中若干文件-右键-New Group from Selection  将选中的文件建组
```

语义
```
option+click        查看变量类型
cmd+click           Jump to Definition
                    Rename    可以保留和main storyboard的元素之间的关联
                    Edit All in Scope
cmd+shift+0         查看Apple文档
cmd+control+arrow   前进,后退
control+I           format indention
```

展开与折叠
```
cmd+option+←    折叠函数体
cmd+option+→    展开函数体
```

## emacs

> [Absolute Beginner's Guide to Emacs](http://www.jesshamrick.com/2012/09/10/absolute-beginners-guide-to-emacs/)

<https://www.coursera.org/learn/programming-languages/supplement/mi5oU/part-a-software-installation-and-use-sml-and-emacs>

> [emacs24-starter-kit](https://github.com/eschulte/emacs24-starter-kit)

### Install

#### mac

> [install guide](https://gist.github.com/devinprater/a794a448ccc46e72fca63c932105c043)

```sh
brew tap railwaycat/emacsmacport
brew cask install emacs-mac

# emacspeak
brew install mplayer
git clone http://www.github.com/tvraman/emacspeak/
cd emacspeak
make config
make emacspeak

vim ~/emacspeak-setup.el
# (load-file "~/emacspeak/emacspeak-setup.el")

make espeak
```

#### debian

```sh
aptitude install emacspeak flite eflite
```

```sh
git clone https://github.com/tvraman/emacspeak
cd emacspeak
make config
make emacspeak
vim ~/emacspeak-setup.el
# (load-file "~/emacspeak/emacspeak-setup.el")
```

### 说明

`C`: 

`C-c |` : control key c key, release, then | key

`C-c C-|` :control key c, release, then control key | key

### 快捷键

`C-x C-c`： 推出emacs
`C-h C-h`	: help

`C-g` 	: quit

`M-x` (run command)

### Buffer

C-x b	打开buffer list 

You can also cycle through buffers sequentially with the key combos `C-x right` and `C-x left`.

Once you are done with the buffer and want to actually close/kill it, use `C-x k`, which will prompt you in the mini-buffer for the name of the buffer to kill (similar to the prompt given when switching buffers). If you don’t specify a buffer, it will kill the active buffer by default.

To open a file and load it into a buffer, use `C-x C-f`

C-x C-s 储存

 `C-x C-w` 另存为

#### Window

Revisiting the concept of windows: they are essentially just views into a buffer. You can open up multiple windows in the same frame (I usually use two vertical windows) and you can have multiple windows displaying the same buffer

There are a few relevant key commands for manipulating windows:

-   `C-x 0` : close the active window（松手之后再按0）
-   `C-x 1` : close all windows except the active window
-   `C-x 2` : split the active window vertically into two horizontal windows
-   `C-x 3` : split the active window horizontally into two vertical windows
-   `C-x o` : change active window to next window

#### Edit

选中：C-space

删除选中区域：C-w. To kill a selected region

删除到尾端：`C-k`.define a region from the point to the end of the current line and kill it with 

粘贴：C-y	 _yank_ is analogous to “paste”. 

撤销：C-/ 

复制：M-w

c-a 跳到行首

c-e 跳到行尾 end

c-f 前进一个字forward

c-b 后退一个字符 back

### 插件

`C-h p`: 打开package manager


## qt

使用国内镜像下载

> http://c.biancheng.net/view/3851.html
> [中科大](http://mirrors.ustc.edu.cn/qtproject/)

> [setup](https://web.stanford.edu/dept/cs_edu/qt-creator/qt-creator-mac.shtml)

> [configure](https://web.stanford.edu/dept/cs_edu/qt-creator/qt-creator-recommended-settings.shtml)

## 原生js和jQuery

方便起见，在表示上，通常用`$()`代替`jQuery()`，通常将jQuery对象命名为`$page`

```js
const page = document.getElementById('page');
const $page = $(page);    // 将原生js的DOM对象转换为jQuery对象
```

### 选择元素

```js
const page = document.getElementById('page');
const articles = page.querySelectorAll('.article')
const article = page.querySelector('.article')  // get first

// jQuery
const $page = $('#page');
const $articles = $page.find('.article');
const $article = $page.find('.article:first');
```

### 创建并挂载元素

```js
const articles = page.querySelector('.container')
const article = document.createElement('p')
article.textContent = '...'
article.classList.add('article')
articles.appendChild(article)

// jQuery
const articles = $('.container');
const article = $('<p>').text('...');
article.addClass('article');
articles.append(article);
```

### 查看元素

```js
const id = article.getAttribute('id')
article.setAttribute('id', 100)
article.id = 100

// jQuery
const id = $(article).attr('id');
$(article).attr('id', 100);
```

### 添加eventListener

```js
links.forEach((link) => {
  link.addEventListener('click', (event) => {
  })
})

// jQuery
$(links).each(function(){
  $(this).on('click', function(event){
  });
});
```

### index.html使用footer.html

```js
const footer = document.getElementById('footer');
fetch('footer.html')
  .then(response => response.text())
  .then(text => {
    footer.innerHTML = text
  })

// jQuery
$("#footer").load("footer.html");
```
