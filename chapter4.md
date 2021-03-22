## 栈

栈结构不是js特有的，需要利用现有的数据类型去封装实现，可以创建一个基于数组的栈，并封装其方法

> 创建栈
```
// 创建一个类来表示栈，需要一种数据结构来保存栈里的元素，此处选择数组
class Stack {
  constructor() {
    this.items = [];
  }
  // 封装栈的方法
  // 添加元素
  push(ele) {
    this.items.push(ele)
  }
  // 移除元素
  pop(ele) {
    this.items.pop(ele)
  }
  // 查看栈定元素
  peek() {
    return this.items[this.items.length - 1]
  }
  // 检查栈是否为空
  isEmpty() {
    return this.items.length === 0
  }
  // 返回栈内元素的个数
  size() {
    return this.items.length
  }
  // 清空栈元素
  clear() {
    this.items = []
    // 或者一直pop知道元素全都移除
  }
}
```

> 使用栈
```
const stack = new Stack()
```

> 优化栈，在最上面创建栈方式不能声明私有属性，下面两种方式可以参考
```
// 下面方法创建了一个假的私有属性
const sitems = Symbol('stackItems')
class Stackone {
  constructor() {
    console.log('symbol----', sitems)
    this[sitems] = []
  }
}

// WeakMap这种数据类型可以确保属性是私有的，采用下面的做法代码可读性不强，扩展该类时无法继承私有属性
const mapitems = new WeakMap()
class Stacktwo {
  constructor() {
    console.log('weakmap----', mapitems)
    mapitems.set(this, [])
  }
}

console.log('new Stackone', new Stackone())
console.log('new Stacktwo', new Stacktwo())
```
