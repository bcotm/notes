# Vim

## vim标记
1. 主旋律：
    1. `dw`
    2. `f{char}`、`d{motion}`、`m{a-zA-Z}`、`<C - r>{register}`
2. 和弦：
    1. `g<C - ]>`

## .命令
重复上次的修改。

例子：
`>G`当前到末尾缩进。
`.`
Line 
    Line
        Line

## 不要自我重复
给每行尾加分号
```java
String a = "haha"
int i = 1
float f = 1.1
```
`A;<ESC>`,`j.j.`

a;  
b;  
c;  

复合型命令

|复合命令|多命令|
|:---:|:---:|
|C|c$|
|s|cl|
|S|^c|
|I|^i|
|A|$a|
|o|A<CR>|
|O|ko|

## 以退为进
感觉模式有点像，移动+修改内容，然后通过.命令来重复修改内容部分的命令，减少操作
\>G 操作+位置
