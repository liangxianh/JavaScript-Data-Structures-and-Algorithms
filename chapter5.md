## 队列和双端队列

1. 队列遵循先进先出的原则

> 创建队列 并添加方法
```
class Queue {
  constructor() {
    this.count = 0 // 用于记录控制队列的大小
    this.lowestCount = 0 // 用于追踪第一个元素
    this.items = {} // 用于存储元素
  }
  // 向队列添加元素
  enqueue(element) {
    this.items[this.count] = element
    this.count++
  }
  // 从队列中删除元素 返回被删除的元素
  dequeue() {
    if (this.isEmpty()) {
      return undefined
    }
    const result = this.items[this.lowestCount]
    delete this.items[this.lowestCount]
    this.lowestCount++
    return result
  }
  // 查看队头元素
  peek() {
    if (this.isEmpty()) {
      return undefined
    }
    return this.items[this.lowestCount]
  }
  // 检查队列是否为空并获取她的长度
  isEmpty() {
    return this.count - this.lowestCount === 0
    // return this.size() === 0
  }
  size() {
    return this.count - this.lowestCount
  }
  // 清空队列
  clear() {
    this.items = {}
    this.count = 0
    this.lowestCount = 0
  }
  // 创建toString方法
  toString() {
    if (this.isEmpty()) {
      return ''
    }
    let objString = `${this.items[this.lowestCount]}`
    for (let i = this.lowestCount + 1; i < this.count; i++) {
      objString = `${objString}, ${this.items[i]}`
    }
    return objString
  }
}
```

> 使用队列
```
const queue = new Queue();
console.log(queue.isEmpty()); // outputs true
queue.enqueue('John');
queue.enqueue('Jack');
console.log(queue.toString()); // John,Jack
queue.enqueue('Camila');
console.log(queue.toString()); // John,Jack,Camila
console.log(queue.size()); // outputs 3
console.log(queue.isEmpty()); // outputs false
queue.dequeue(); // remove John
queue.dequeue(); // remove Jack
console.log(queue.toString()); // Camila
```

2. 双端队列,可以在队前插入元素，页可以在队尾删除元素

> 创建Deque类，和queue不同的是可以在队前后取添加删除查看元素
```
// 以在队首添加元素为例，有三种情况
  // 1 空 直接插入元素
  // 2 队首元素 > 0直接插入元素
  // 3 对手元素key为0， 所有元素向后移位，空出首为直接添加元素
  addFront(element) {
    if (this.isEmpty) {
      this.addBack(element)
    } else if (this.lowestCount > 0) {
      this.lowestCount--;
      this.items[this.lowestCount] = element
    } else {
      for (let i = this.count; i > 0 ; i++) {
        this.item[i] = this.item[i-1]
      }
      this.count++
      this.lowestCount = 0;
      this.items[0] = element
    }
  }
```

3. 使用队列解决问题
> 击鼓传花游戏
```
const { Queue } = PacktDataStructuresAlgorithms;

function hotPotato(elementsList, num) {
  const queue = new Queue()
  const elimitatedList = []

  for (let i = 0; i < elementsList.length; i++) {
    queue.enqueue(elementsList[i])
  }

  while (queue.size() > 1) {
    // num 可以随机
    // let num = Math.ceil(Math.random(1,30) * 30)
    for (let i = 0; i < num; i++) {
      queue.enqueue(queue.dequeue())
    }
    elimitatedList.push(queue.dequeue())
  }

  return {
    eliminated: elimitatedList,
    winner: queue.dequeue()
  }
}
const names = ['John1', 'Jack2', 'Camila3', 'Ingrid4', 'Carl5'];
const result = hotPotato(names, 7);

result.eliminated.forEach(name => {
  console.log(`${name} was eliminated from the Hot Potato game.`);
});

console.log(`The winner is: ${result.winner}`);

// Camila3 was eliminated from the Hot Potato game.
// Jack2 was eliminated from the Hot Potato game.
// Carl5 was eliminated from the Hot Potato game.
// Ingrid4 was eliminated from the Hot Potato game.
// The winner is: John1
// 上例子中每次设置了固定个数的传递，会存在预测性，如果每次的num不通过呢？
```



