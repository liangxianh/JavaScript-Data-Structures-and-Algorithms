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
