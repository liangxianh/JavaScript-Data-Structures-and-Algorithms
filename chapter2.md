## ECMAScript he Typescipt概述

### ECMASscript简单介绍
1. export import
```
当导出只有一个默认是 export default class Book
引入时 直接引入即可 不需要{} 多个时需要需要{}
```

### Typescript
1. 类型判断
```
let age:number = 20
let age = 20
let age:number;
```
2. 接口
```
// 接口
interface Person {
  name: string,
  age: number
}
function printName(person: Person) {
  console.log('person.name')
}
const john = { name: 'john', age: 21 }
const mary = { name: 'mary', age: 12, phone: '021-1234545656' }
printName(john)
printName(mary)
const errorP = { name: 1, age: 13 }
printName(errorP)  //此行在编译时将报错
```
上面ts 转换为js只是普通的js
```
function printName(person) {
    console.log('person.name');
}
var john = { name: 'john', age: 21 };
var mary = { name: 'mary', age: 12, phone: '021-1234545656' };
printName(john);
printName(mary);
var errorP = { name: 1, age: 13 };
printName(errorP);
```
但是在上面的代码中最后一行，编译时会报错
```
datatype.ts(25,11): error TS2345: Argument of type '{ name: number; age: number; }' is not assignable to parameter of type 'Person'.
  Types of property 'name' are incompatible.
    Type 'number' is not assignable to type 'string'.
```
<b>注意：代码补全以及类型和错误检查只在编译时时可用的</b>

3. 泛型,用尖括号向Comparable接口动态传入T类型，可以指定compareTo函数的参数类型
```
interface Comparable<T> {
  compareTo(b: T): number;
}
```
### Typescript中对javascript文件的编译时检查
1. 在计算机上全局安装ts
2. 在js文件<b>第一行<b>添加//@ts-check
```

```

