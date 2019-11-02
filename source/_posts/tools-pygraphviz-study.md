---
title: linux
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-11-01 13:28:11
tags:
- python
categories:
- tools
---

参考
1. [Node, Edge and Graph Attributes of Pygraphviz](http://graphviz.org/doc/info/attrs.html)
2. [graphviz 程序生成多种类型图表详解](https://www.cnblogs.com/liang1101/p/7641984.html)


## I 一个完整的pipline
1. install
    ```
    sudo apt-get install python-dev graphviz libgraphviz-dev pkg-config
    ```
2. 初始化
    - .dot文件 `dot hello.dot -T png -o hello.png`
    ```
    strict digraph{
        graph [bb="-64.382, -82.544, 57.743, 78.934",
            center=True,
            label=wyp_label];
        node [label="\n"];
        edge [label="\n"]
        1   [height=0.5,
            pos=", "]
        2   []
        1 -> 2 [pos=""]
    }
    ```
    - dict `d = {'1': {'2': None}, '2': {'1': None, '3':None}, '3':{'2': None}}  A=pgv.AGraph(d)`
    - 图形
        > man <cmd>?
        - dot 明确的方向性
        - neato 缺乏反向性
        - twopi 放射性
        - circo 环形布局
    - pipline
        ```
        import pygraphviz as pgv
        # 1. init a graph
        G = pgv.AGraph(strict=, directed=) # default: strict-True
        
        # 2. set the global property
        G.graph_attr[''] = ''
        G.node_attr['']=''
        G.edge_attr['']=''

        # 3. add node and edge
        G.add_node_from(list1)
        G.add_node('a', color='red')
        G.add_edge('b', 'c', color='blue')

        # change node or edge attributes
        n = G.get_node('f')
        n.attr['shape']='box'
        e = G.get_edge('b', 'c')
        e.attr['color']='green'

        # 4. set layout: dot(direction), twopi(away), circo(circle)
        G.layout(lay)
        
        # 5. save fig
        G.draw(file_path)
        ```
    - 属性
        - node
            - color
            - comment: ??
            - fillcolor
            - fontname: font family Times-Roman
            - fontsize: 14
            - label
            - shape
        - edge
            - arrowhead: normal
            - arrowsize: 1.0
            - arrowtail
            - color
            - comment
            - dir: forward, back, both
            - fontcolor: black; fontname: ; fontsize
            - headlabel: label near head of edge
            - label: edge label
            - labelfontcolor:; labelfontname:;
        - graph
            - bgcolor: 
            - center false
            - comment: 
            - label
                > 有意思的是支持html语言
                > http://graphviz.org/doc/info/shapes.html#record
                > https://www.cnblogs.com/zhishaofei/p/4033175.html
    - 其他的
        - 转换为pvg
            `h=A.handle; C=pgv.AGraph(h);A.draw("a.svg");`
        - load and save dot file
            ```
            G = pgv.AGraph(dot_file)
            G.write(dot_file)
            ```
        - string
            > s = G.string()

## 待做

