---
title: lua learning
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-10-02 23:58:53
tags:
- gnn
categories:
---
https://www.runoob.com/lua/lua-miscellaneous-operator.html

1. data struture:
    - nil
    - boolean
    - number: 双精度的实浮点数
    - string
    - funcion: 由C或lua编写的函数
    - userdata: 任意存储在变量中的C数据解雇
    - thread: 表示执行的独立线路，用于执行协同程序
    - table: 关联数据
    > type() 查看类型
2. variable
    - global
    - local
    - 表中的索引 A.b, A[b], gettable_event(t, i)
3. loop
    1. while (true) \ do \ 执行体 \ end
    2. do () while() end
    3. for
        - 数值
            > for var=exp1, exp2, exp3 do \ 执行体 \ end
        - 泛型循环 相当于foreach
            > for i, v in ipairs(a) do \ print(i, v) \ end
            > for k, v in pairs(table) do \ print(k, v) \ end

4. if: process control
    - if(cond) then \ true-exp \ end
    - if(cond) then \ true-exp \ else \ false-exp \ end
    - if(cond1) the \ true1-exp \ elseif(cond2) \ true2-exp \ else \ else-exp \ end
5. function
    - 定义
        > `<scope> function <function-name> (arg1, arg2, ..., argn) \ <function-body> \ return result_params_comma_separated \ end`
    - usage
        - add(...) 三点表示可变参数 
6. 运算符
    - +, -, *, /, %, ^, -
    - ==, ~=, >, >=, <=
    - and, or, not
    - a..b,  #a
7. string
    > 以下均是静态方法
    - uppper
    - lower
    - gsub(mainstr, findstr, replacestr, num)
    - find(str, substr, [init, [end]]): index
    - reverse
    - format("the value is:%d", 4)
        - %c, %d, %f, %s
        - [符号][占位符][对齐标志-左对齐][宽度数值][小数位数]
        > more: https://www.runoob.com/lua/lua-strings.html
    - char(arg): convert other type to char type to concat
    - byte(arg)
    - len(str) 
    - rep(str, n): 返回n个拷贝
    - gmatch(str, re_pattern): return iterator
    - match(str, re_pattern): return the first match



? lua have not go to definition??

th TREPL都充满了方便的特性:
1. tab不全
2. 历史 history
3. 打印 print
4. eval() 自动打印
5. 自助:?
6. shell命令: $ ls


torch中文网学习
https://ptorch.com/docs/2/five-simple-examples