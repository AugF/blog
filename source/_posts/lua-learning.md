---
title: lua learning
copyright: true
top: 0
reward: false
mathjax: true
date: 2019-10-02 23:58:53
tags:
categories:
---

## 1. Variables and flow control
```
num = 42
> all is doules 双精度， 64bit,  <=53bit is not problem

no ++ or +=


while ..  do ... end

if ...  then .. elseif .. else ... end

if not BoolValue the ... end

for i=1, 100, step do ... end

repeat
    print('the way of the future')
util ...
```

## 2. Functions
```
function fib(n)
    if n < 2 then return 1 end
    return fib(n-2) + fib(n-1)

> closures and anonymous functions are ok.

function adder(x)
    return function (y) return x+y end
end

x, y, z = 1, 2, 3, 4
> 4 is discarded

function bar(a, b, c)
    print(a, b, c)
    return 4, 8, 15, 15, 23, 43
end

x, y = bar('zerap')

function f(x) return x*x end
f = function (x) return x*x end

local function g(x) return math.sin(x) end
local g; g = function(x) return math.sin(x) end
> local decl makes g-self-references ok ???
print 'hello'
```

## 3. Tables
lua's only compound data structure.
hash-lookup dicts  --- lists
tables  --  dicts, maps

```
t = {key1 = 'value1', key2 = false}
print(t.key1)
t.newKey={}
t.key2 = nil // removes key2 from the table
```