# Python数据类型

## 标准类型
## 数字

- 整型，int
    + 布尔，bool
    + 长整型，long
- 浮点，float
- 复数，complex

### 算数/逻辑运算

1. 系统决定整型还是浮点型。
2. 除法：py2地板除，py3精确。
3. 可重载对象\_\_nonzero()\_\_改变bool(obj)。


## 字符串

py2:str，字节
py3:str，Unicode

1. py2使用unicode()而不是str();
2. 程序中出现字符串都加u;
3. 除读写文件、网络传输、数据库，不编解码。

运行时连接字符串：
"%s %s"%('a', 'b')
"".join(("a", "b"))

原始字符串
r"\t"

包含换行符
"""

## 列表

### 访问方式
1. 单个元素，通过偏移量。
2. 多个元素，通过切片。

### 操作符
1. seq[idx]
2. seq[beg\:end\:step]
3. seq*expr
4. seq1+seq2
5. obj in seq

支持切片运算
## 元组
## 字典