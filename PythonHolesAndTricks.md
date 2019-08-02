1. 空格或tab统一；

2. 可变对象不可做默认参数；
3. 遍历迭代器不可删除添加内容；
4. 目录下\_\_init\_\_.py才会被视为packge，并导入目录下文件；
5. 深拷贝copy.deepcopy()；
6. 乘法复制是浅拷贝，a=[[]]*3 a[0].append(1)；
7. 修改全局变量，如果是rebinding，需要global关键字，如果是mutation则不用；