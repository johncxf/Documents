### vi/vim

#### 模式

##### 标准模式

可以使用快捷键命令，或按:输入命令行

##### 插入模式

可以输入文本，在正常模式下，按i、a、o等都可以进入插入模式

##### 可视模式

正常模式下按v可以进入可视模式， 在可视模式下，移动光标可以选择文本。按V进入可视行模式， 总是整行整行的选中。ctrl+v进入可视块模式

##### 替换模式

正常模式下，按R进入

#### 启动

- vim -c cmd file: 在打开文件前，先执行指定的命令；
- vim -r file: 恢复上次异常退出的文件；
- vim -R file: 以只读的方式打开文件，但可以强制保存；
- vim -M file: 以只读的方式打开文件，不可以强制保存；
- vim -y num file: 将编辑窗口的大小设为num行；
- vim + file: 从文件的末尾开始；
- vim +num file: 从第num行开始；
- vim +/string file: 打开file，并将光标停留在第一个找到的string上。
- vim –remote file: 用已有的vim进程打开指定的文件。 如果你不想启用多个vim会话，这个很有用。但要注意， 如果你用vim，会寻找名叫VIM的服务器；如果你已经有一个gvim在运行了， 你可以用gvim –remote file在已有的gvim中打开文件

#### 常用命令

##### 保存文件

| 命令    | 说明           |
| ------- | -------------- |
| :w      | 保存保存       |
| :w file | 另存为文件     |
| :q      | 退出           |
| :q!     | 放弃修改退出   |
| :wq     | 保存修改并退出 |

##### 编辑文件

| 按键 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| `i`  | 在当前光标字符前插入                                         |
| `a`  | 在当前光标字符后插入                                         |
| `I`  | 在当前行首插入                                               |
| `A`  | 在当前行尾插入                                               |
| `o`  | 在下方开一新行，插入                                         |
| `O`  | 在上方开一新行，插入                                         |
| `r`  | 替代字符，将当前字符替代为紧跟着输入的字符                   |
| `R`  | 进入替代模式，将当前及之后的字符都替代为紧跟着输入的字符串，直到按 `` 返回 Normal 模式 |

##### 移动命令

| 按键 | 描述                            |
| ---- | ------------------------------- |
| `h`  | 光标左移                        |
| `j`  | 光标下移                        |
| `k`  | 光标上移                        |
| `l`  | 光标右移                        |
| `0`  | 跳到行首，可以理解为无穷大的`h` |
| `^`  | 跳到行首开始的第一个非空白字符  |
| `$`  | 跳到行尾，可以理解为无穷大的`l` |
| `gg` | 跳到首行，可以理解为无穷大的`k` |
| `G`  | 跳到末行，可以理解为无穷大的`j` |
| `w`  | 跳到下一个词首                  |
| `ge` | 跳到上一个词尾                  |
| `b`  | 跳到单词开头                    |
| `e`  | 跳到单词尾部                    |
| 5k   | 向上移动5行                     |
| 5j   | 向下移动5行                     |
| 5w   | 向后移动5个词                   |
| fx   | 向前移动到字符x上               |
| Fx   | 向后移动到字符x上               |
| tx   | 移动到字符x前                   |
| Tx   | 向后移动到字符x前               |

##### 删除命令

| 按键 | 说明                                |
| ---- | ----------------------------------- |
| x    | 删除当前光标所在处的字符            |
| X    | 删除当前光标左边的字符              |
| d$   | 删除从光标到一行末尾的整个文本      |
| d0   | 删除从光标到一行开头的所有单词      |
| dd   | 删除当前光标处的一整行              |
| 5dd  | 删除从光标开始处的5行代码           |
| dgg  | 删除从光标到文本开头                |
| dG   | 删除从光标到文本结尾                |
| di"  | 删除在引号之间的内容                |
| dit  | 删除HTML标签内容                    |
| dtx  | 向后删除字符直到遇到第一个 `x` 字符 |
| dw   | 删除到下一个单词的词首              |
| dW   | 删除到右边界                        |
| daw  | 删除到左或边界，适合删除HTML属性    |
| diw  | 删除光标所在单词                    |
| daw  | 删除光标所在单词，包括空格          |
| D    | 删除到行尾                          |
| C    | 删除到行尾，进入插入模式            |
| c^   | 删除到行首                          |
| c$   | 删除到行尾                          |

##### 复制粘贴

| 按键 | 说明                          |
| ---- | ----------------------------- |
| y    | 复制                          |
| p    | 贴                            |
| yy   | 复制一整行                    |
| 2yy  | 复制从当前光标所在行开始的2行 |
| yit  | 复制标签内容                  |
| yat  | 复制完整标签                  |
| yG   | 复制到最后                    |
| y{   | 复制到段落开始                |
| y}   | 复制到段落结尾                |

##### 修改替换

| 按键 | 说明                                                         |
| ---- | ------------------------------------------------------------ |
| cw   | 删除从光标处到单词结尾的文本并进入到插入模式                 |
| cb   | 删除从光标处到单词开头的文本并进入到插入模式                 |
| cc   | 删除一整行并进入到插入模式                                   |
| r    | 替换当前光标下的字符                                         |
| R    | 进入到替换模式                                               |
| ctx  | 向后修改内容到 `x`，也就是意味着删除光标到 `x` 中间内容，并进入插入模式 |
| ci"  | 改写双引号中的内容                                           |
| cc   | 编辑当前行                                                   |
| c^   | 删除到行首，并进入编辑模式                                   |
| c$   | 删除到行尾，并进入编辑模式                                   |

##### 注释操作

| 按键 | 说明     |
| ---- | -------- |
| gcc  | 注释     |
| gc2j | 注释两行 |

##### 查找替换

| 按键     | 说明                         |
| -------- | ---------------------------- |
| :/string | 正向查找                     |
| :?string | 反向查找                     |
| fx       | 向后查找x字符，按 ; 继续查找 |
| Fx       | 向前查找x字符，按 ; 继续查找 |

##### 快速跳转

| 按键 | 说明                    |
| ---- | ----------------------- |
| gg   | 跳转到第一行            |
| G    | 跳转到最后一行          |
| 5G   | 跳到第5行               |
| :5   | 跳转到第5行（命令模式） |
| {    | 到段首                  |
| }    | 到段尾                  |

##### 撤销命令

| 按键   | 说明                 |
| ------ | -------------------- |
| u      | 撤销上一步的操作     |
| ctrl+r | 将原来的插销重做一遍 |

##### 窗口管理

| 命令                | 说明                         |
| ------------------- | ---------------------------- |
| :vs 或 :vsplit      | 左右分屏                     |
| :sp 或 :split       | 上下分屏                     |
| :vertical resize 80 | 设置宽度 80%                 |
| :resize 80          | 设置高度 80%                 |
| ctrl+w c            | 关闭当前窗口                 |
| ctrl+w o            | 关闭其他窗口                 |
| ctrl+w l            | 切换左边窗口                 |
| ctrl+w j            | 切换上边窗口                 |
| ctrl+w k            | 切换下面窗口                 |
| ctrl+w l            | 切换右面窗口                 |
| ctrl+w w            | 窗口间循环切换               |
| ctrl+w x            | 窗口互换                     |
| ctrl+w H            | 从**水平**布局**到垂直**布局 |
| ctrl+w J            | 从**垂直**布局**到水平**布局 |

### Neovim

`Neovim`是基于vim的速度更快的编辑器，也是vim的良好替代品。官网：https://neovim.io/

#### 安装

**mac**

```
brew install neovim
```

#### 配置

创建配置文件用于设置neovim配置

```text
mkdir -vp ~/.config/nvim 
touch ~/.config/nvim/init.vim
```

安装一些插件需要让VIM支持PYTHON

```text
pip3 install --user --upgrade neovim
```

#### 插件

插件安装一般使用 vim-plug 管理，基本步骤如下

- 以下代码中以 `Plug` 开始的要放在 `~/.config/nvim/init.vim` 文件中
- 执行 `source $INITVIM` 生新加载配置
- 在 neovim 中执行命令 `:PlugInstall` 进行安装

##### 常用插件管理命令

| 命令                                | 描述                                 |
| ----------------------------------- | ------------------------------------ |
| `PlugInstall [name ...] [#threads]` | 安装插件                             |
| `PlugUpdate [name ...] [#threads]`  | 安装或更新插件                       |
| `PlugClean[!]`                      | 删除未列出的插件                     |
| `PlugUpgrade`                       | 本身升级vim-plug                     |
| `PlugStatus`                        | 检查插件状态                         |
| `PlugDiff`                          | 检查来自先前更新的更改以及未决的更改 |
| `PlugSnapshot[!] [output path]`     | 生成脚本以还原插件的当前快照         |

##### vim-plug

###### 安装

官网：https://github.com/junegunn/vim-plug

```text
curl -fLo ~/.local/share/nvim/site/autoload/plug.vim --create-dirs \
    https://houdunren-video.oss-cn-hangzhou.aliyuncs.com/soft/plug.vim
```

###### 配置文件

在 `~/.vimrc` 或 `~/.config/nvim/init.vim` 文件中定义 vim-plug 的配置

###### 安装插件

1. 下面是 vim.plug 的示例，在`call plug#begin` 与 `call plug#end()` 间定义插件

   ```text
   call plug#begin('~/.vim/plugged')
   
   Plug 'StanAngeloff/php.vim'
   Plug 'shawncplus/phpcomplete.vim'
   Plug 'neoclide/coc.nvim', {'branch': 'release'}
   
   Plug 'mhinz/vim-startify'
   
   call plug#end()
   ```

2. 重新启动nvim并执行命令

   ```text
   :PlugInstall
   ```

3. 如果出现以下错误

   ```text
   startify: Can't read viminfo file. Read :help startify-faq-02
   ```

   执行以下命令

   ```text
   chmod 777 ~/.viminfo
   ```

##### coc.nvim

Coc是 Vim / Neovim的智能感知引擎，提供像vscode类似的提示功能，你可以通过 [查看文档](https://github.com/neoclide/coc.nvim/wiki) 了解全部。

###### 安装

在 `~/.config/nvim/init.vim` 文件中添加

```text
Plug 'neoclide/coc.nvim', {'branch': 'release'}
```

回到 neovim中执行安装

```text
:PlugInstall
```

###### 插件安装

coc.vim也提供了插件管理功能，官方插件列表 https://github.com/neoclide/coc.nvim/wiki/Using-coc-extensions#implemented-coc-extensions

- `.config/coc/extensions/package.json` 为插件安装列表

插件安装在以下目录

```text
cd ~/.config/coc/extensions
```

常用开发语言配置 https://github.com/neoclide/coc.nvim/wiki/Language-servers#register-custom-language-servers

下面以安装 `coc-json coc-phpls` 两个插件为例

1. 安装 `:CocInstall coc-json coc-phpls`
2. 查看插件列表 `:CocList extensions`
3. 删除插件 `:CocUninstall coc-phpls`，无法删除 `vim-plug` 安装的插件

###### 插件配置

有些插件有独立的配置项，在 `neovmi` 中执行命令 `:CocConfig` 打开配置文件，并添加上所安装插件的独立配置项

##### 常用插件

###### coc-phpls

> php 语言的代码提示插件

需要先安装 `Intelephense`

```text
$ npm i intelephense -g
```

然后安装插件

```text
:CocInstall coc-phpls
```

需要通过命令 `:CocConfig` 进行配置

```text
{
  "languageserver": {
    "intelephense": {
      "command": "intelephense",
      "args": ["--stdio"],
      "filetypes": ["php"],
      "initializationOptions": {
        "storagePath": "/tmp/intelephense"
      }
    }
  }
}
```

如果你有 `Intelephense` 付费密钥，在 `coc-settings.json` 文件中添加，使用 `:CocConfig` 可编辑该文件

```text
{
    "intelephense.licenceKey": "your licence key",
}
```

