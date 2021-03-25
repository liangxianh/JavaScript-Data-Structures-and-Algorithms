## 集合 set

> 集合是由一组无序且唯一的项组成的
> 创建集合类并添加方法
```
// 创建集合类,用对象来实现（实际也可以用数组实现），
class Set {
  constructor() {
    this.items = {}
  }
  has(element) {
    // return element in this.items
    // 更好的实现方式
    // in操作符用于判断某个属性属于某个对象，可以是直接属性，也可以是通过prototype继承的属性，即用in返回的是原型链上是否有改属性的布尔值；而hasOwnProperty表明对象是否有该属性
    return Object.prototype.hasOwnProperty.call(this.items, element)
    // 注意：上面之所以用call 是由于Object.prototype对象的hasOwnProperty方法也有可能会被覆盖
  }
  add(element) {
    if (!this.has(element)) {
      this.items[element] = element
      // 返回true表明已经添加了该元素
      return true
    }
    // 返回false 表明集合中已经有了该元素
    return false
  }
  delete(element) {
    if (this.has(element)) {
      delete this.items[element]
      // 返回true表明已经添加了该元素
      return true
    }
    return false
  }
  clear() {
    this.items = {}
  }
  // Object.keys 返回一个包含给定对象所有属性的数组
  size() {
    return Object.keys(this.items).length
  }
  // 另一种size方式
  sizeLegacy() {
    let count = 0
    for (let key in this.items) {
      if (this.items.hasOwnProperty(key)) {
        count++
      }
    }
    return count
  }
  // Object.values 返回一个包含给定对象所有属性值的数组 ES2017语法
  values() {
    return Object.values(this.items)
  }
  valueLegacy() {
    let values = []
    for (let key in this.items) {
      if (this.items.hasOwnProperty(key)) {
        // values.push(key) //此方法将会把keypush进入values 若为int整形数据 则此操作会变成字符串
        values.push(this.items[key])
      }
    }
    return values
  }
}
```
> 使用set
```
const set = new Set();

set.add(1);
console.log(set.values()); // outputs [1]
console.log(set.has(1)); // outputs true
console.log(set.size()); // outputs 1

set.add(2);
console.log(set.values()); // outputs [1, 2]
console.log(set.has(2)); // true
console.log(set.size()); // 2

set.delete(1);
console.log(set.values()); // outputs [2]

set.delete(2);
console.log(set.values()); // outputs []
```

