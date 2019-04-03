# Vim Basic
本质就是对文本的修改与删除。
Vim tutor
## cursor

## change   
    operator [number] motion

## find/substitude
:s/old/new/g
:#,#s/old/new/g
:%s/old/new/g

## command
!ls

## option
set ic, is, hls
~/.vimrc

## help
:help user-manual


## 模式
- insert
- normal
- visual
- replace

i I
a A
o O
r R
v 

## 文本修改
**操作符**+**重复次数**+**移动**
**Operator**+**Number**+**Motion**

|Operator| 说明 |
|:-:|:-|
| d |删除|
| c |改变|
| y |复制, yank into registers|
| ~ |改变大小写|
| gu | 转换为小写 guaw |
| gU | 转换为大写 |
| p |put after cursor, above where the content should go|
| < | 增加缩进 |
| > | 减小缩进 |
| = | 自动缩进 |
| r |替换|
| x |删除|
| s ||

// 作用范围, character,lines,blocks

|Motion|说明|
|:-:|:-|
| w |直到下个单词第一个字符|
| p | 段落 |
| e |当前单词末尾|
| b ||
| $ |行(尾)|
| 0 ||
| % |match ), ], }|
| **^** |**非空行首**|
| v ||
|aw|a word|
|as|a sentence|

## 复合型命令

|复合命令|多命令|说明|
|:---:|:---:|:-:|
|**C**|c$|删除光标到行尾内容，进入insert模式|
|s|cl|删除一个字符，进入insert模式|
|**S**|^c|删除整行内容，进入insert模式|
|I|^i|跳至非空行首，进入insert模式|
|A|$a|跳至行尾append，进入insert模式|
|o|A<CR>||
|O|ko||

gg
G
CTRL-i
CTRL-o

## 查找和替换

|命令|说明|
|:-|:-|
|f<Char\>|行内查找字符|
|f<Char\> **;,**|向后、前查找|
|:/pattern<CR\>|全文查找|
|:?pattern<CR\>|全文反向查找|
|:s/target/replacement<CR\>|替换|
|.**&, u**|继续替换|
|:s/target/replacement/**g**<CR\>|全行替换|
|:**#,#**s/target/replacement/**g**<CR\>|行替换|
|:**%**s/target/replacement/**g**<CR\>|全文替换|


## 屏幕移动

|命令|说明|
|:-|:-|
|zz|当前行移动到屏幕中央|
|zt|当前行移动到屏幕顶部|
|zb|当前行移动到屏幕底部|

# VIMTUTOR到6.1章节
## 文件操作
v :w XXX
:!pwd
:r XXX

## 配置
:set ignorecase(ic)
:set hlsearch(hls), incsearch(is)
:set noxxx
:set nocp


## 帮助
<F1\>
:help <CR\>
:help user-manual
