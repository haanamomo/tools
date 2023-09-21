## git

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

```
git push origin v1.5
git push origin --tags
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

### 查看 git show

```sh
git show <commit_id>
```

### 删除 git rm/clean

```sh
git rm --cached <file>  # 移除追踪，不删除
git rm <file>  # 删除文件并git add

git clean -f  # 删除untracked files
```

### 改变本地repo版本 git reset

```sh
# 首先将本地的commit恢复到<commit_id>
git reset <commit_id> <file>  # 将<commit_id>以后的改动放到work区
git reset --soft <commit_id> <file> # 将<commit_id>以后的改动放到add区
git reset --hard <commit_id>  # 将<commit_id>以后的改动移除
```

## Sublime

打开链接：选中链接，右键在浏览器中打开

移动行：mac `cmd + control + arrow`, win `shift + control + arrow`

删除行：`shift + control + k`

在选中的行尾插入：`control + shift + l`

多重选中：`meta + d`

选中所有：mac `control + cmd + g`, win `alt + f3`

隐藏边栏：`meta+k, meta+b`

显示函数列表或Markdown目录：`meta + r`


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

1.  安装Package Control
	Open the command palette
	Win/Linux: ctrl+shift+p, Mac: cmd+shift+p
	Type Install Package Control, press enter

2. cmd+shift+p , 输入 Install Package

推荐插件：
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

分成1列/2列/3列/4列/网格：`cmd+option+1/2/3/4/5` 

焦点移动到第1/2/3/4列：`control+1/2/3/4` 

将当前文件移动到第1/2/3/4列：`shift+control+1/2/3/4` 

### 在侧栏显示当前文件

需要在Keybinding里定义

```
{ "keys": ["ctrl+r"], "command": "reveal_in_side_bar" }
```

## vscode

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
