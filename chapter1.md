## 简介

### 变量

1. 数据类型

> 原始数据类型 undefined/null/数/字符串/布尔值、symbol

> 派生数据类型 js对象 函数/数组/日期和正则表达式

```
symbol 标记/标志/象征
// 唯一标识符，可用作对象的唯一属性值

// Here are two symbols with the same description:

let Sym1 = Symbol("Sym")

let Sym2 = Symbol("Sym")

console.log(Sym1 === Sym2)// returns "false"// Symbols are guaranteed to be unique.// Even if we create many symbols with the same description,// they are different values.

let Sym = Symbol("Sym")

console.log(Sym.toString())    // Symbol(Sym), now it works

let _Sym = Symbol("Sym");

console.log(_Sym.description);    // Sym

Symbol.keyFor(Symbol.for("tokenString")) === "tokenString"    // true
```
