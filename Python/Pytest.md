
## 从使用看设计

### 命令行使用方式

1. 执行，`pytest`, `python -m pytest`
2. 统一返回，返回码共6个。
    - 是否收集成功
    - 是否执行通过
    - 是否被用户中断
    - 是否发生内部错误
    - 命令行使用错误

### 执行测试

1. 开始位置，不同维度
    - 模块
    - 目录
    - keyword表达式
    - 收集的测试nodeid，`test_mod.py::TestClass:test_method`
    - marker
    - packages
2. 调试相关
    - 修改traceback打印信息
    - 执行失败进入pdb进行调试
3. Assert相关
    - 展示执行失败时表达式的值，调用、属性、比较、二进制以及一元运算等
4. 报告定制
    - 各种报告数据
    - 各种报告格式
    - 执行数据，耗时分析

### 支持插件

1. fixtures
2. 
