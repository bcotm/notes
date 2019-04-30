# Vim

## 解决问题的方式-优化重复性操作
## vim标记
1. 主旋律：
    1. `dw`、`dap`
    2. `f{char}`、`d{motion}`、`m{a-zA-Z}`、`<C-r>{register}`
2. 和弦：
    1. `g<C-]>`

## .命令
重复上次的修改。一键移动，一键修改。  

## *命令
查找当前光标下的单词。

## 实际操作例子
### 例子1
**把每行依次缩进。**
```
Line one
Line two
Line three
```
改成
```
    Line one
        Line two
            Line three
```
### 方案
```java
>G // 当前到末尾缩进。  
j.j. // 下一行，重复操作。
```

### 例子2
**给每行尾加分号**
```java
String a = "haha"
int i = 1
float f = 1.1
```
```java
String a = "haha";
int i = 1;
float f = 1.1;
```
### 方案
```java
A;<ESC> // 在行尾添加;并返回insert模式。  
j.j. // 下一行，重复操作。
```

### 例子3
**在字符前后添加空格**
```java
// 在"+"前后增加空格。
method(arg+arg1+arg2);
method(arg + arg1 + arg2);
```
### 方案
```java
f+ //行内查找+号。  
s + <ESC> // 删除当前字符并进入insert模式，输入空格+空格。  
;. // 继续查找，重复操作。
```
### 例子4
**修改数字，可以通过加减**
```javascript
.bolg,.news{background-image:url(/sprite.png);}
.bolg,{background-positoin:0px 0px;}
```
```javascript
.bolg,.news{background-image:url(/sprite.png);}
.bolg {background-positoin:0px 0px;}
.news {background-positoin:180px 0px;}
```
### 方案
```java
yyp //复制新行
jwcw news 
180<c+a> //自动查找第一个数字并增加180
```
### 例子5
**删除不只一个单词**
```java
Delete more than one word.
```
```java
Delete one word.
```
### 方案
```java
w
dw. //能够重复，就不要使用次数
```
### 例子6
```java
Delete more than one word.
```
```java
Delete the word.
```
### 方案
```java
w
c3w //需要修改的，用次数比较方便
```
### 例子7
**大写整个单词**
```java
java
```
```java
JAVA
```
### 方案
```java
gUaw
```
### 例子8
**把第一行的书名复制到第二行末尾**
```java
Practical Vim, by Drew Neil 
Read Drew Neil's 
```
```java
Practical Vim, by Drew Neil 
Read Drew Neil's Practical Vim
```
### 方案
```java
yt,
jA
<c-r>0 复制寄存器0中的内容
```


让 **移动** 和 **修改** 能都够重复。
**.范式：**一次按键移动，一次按键修改。
插入模式下 ↑↓←→都会产生一个新的撤销快，也会对.命令产生影响。

## 查找并手动替换
1. 查找单词："*"
2. 手动替换："cwsub<ESC>"
3. 重复：n.n.n.n

## 画家停顿离开画笔，进入普通模式

## 撤销单元切成块
u会撤销最新的对文本内容的修改。所有可以控制好自己修改的粒度。

## 构造可重复修改
> 当前光标位于行内最后一个字符，要删除一个单词
1. dbx
2. bdw
3. daw
最符合.命令重复操作的是3。
