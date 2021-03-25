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

> 集合的运算
```
// 并集
  union(otherSet) {
    const unionSet = new Set()
    this.values().forEach(value => unionSet.add(value))
    otherSet.values().forEach(value => unionSet.add(value))
    return unionSet
  }
  // 交集
  intersection(otherSet) {
    const intersectionSet = new Set()
    const values = this.values()
    for(let i = 0; i < values.length; i++) {
      if (otherSet.has(values[i])) {
        intersectionSet.add(values[i])
      }
    }
    return intersectionSet
  }
  // 差集
  difference(otherSet) {
    const differenceSet = new Set()
    const values = this.values()
    this.values().forEach(value => {
      if (!otherSet.has(value)) {
        differenceSet.add(value)
      }
    })
    // for(let i = 0; i < values.length; i++) {
    // }
    return differenceSet
  }
  // 子集
  isSubsetOf(otherSet){
    if (this.size() > otherSet.size()) {
      return false
    }
    let isSubset = true
    this.values().every(value => {
      if (!otherSet.has(value)) {
        isSubset = false
        return false
      }
      return true
    })
    return isSubset
  }
```
```
// --------- Union ----------

let setA = new Set();
setA.add(1);
setA.add(2);
setA.add(3);

let setB = new Set();
setB.add(3);
setB.add(4);
setB.add(5);
setB.add(6);

const unionAB = setA.union(setB);
console.log(unionAB.values()); // [1, 2, 3, 4, 5, 6]

// --------- Intersection ----------

setA = new Set();
setA.add(1);
setA.add(2);
setA.add(3);

setB = new Set();
setB.add(2);
setB.add(3);
setB.add(4);

const intersectionAB = setA.intersection(setB);
console.log(intersectionAB.values()); // [2, 3]

// --------- Difference ----------

setA = new Set();
setA.add(1);
setA.add(2);
setA.add(3);

setB = new Set();
setB.add(2);
setB.add(3);
setB.add(4);

const differenceAB = setA.difference(setB);
console.log(differenceAB.values()); // [1]

const differenceBA = setB.difference(setA);
console.log(differenceBA.values()); // [4]
// --------- Subset ----------

setA = new Set();
setA.add(1);
setA.add(2);

setB = new Set();
setB.add(1);
setB.add(2);
setB.add(3);

const setC = new Set();
setC.add(2);
setC.add(3);
setC.add(4);

console.log(setA.isSubsetOf(setB)); // true
console.log(setA.isSubsetOf(setC)); // false
```

> ECMAScript2015 Set类, 注意使用时需要babel转换
> ECMAScript2015 Set存储的时size属性，values返回的时Iterator
```
const set = new Set();

set.add(1);
console.log(set.values()); // outputs @Iterator
console.log(set.has(1)); // outputs true
console.log(set.size); // outputs 1

set.add(2);
console.log(set.values()); // outputs [1, 2]
console.log(set.has(2)); // true
console.log(set.size); // 2

set.delete(1);
console.log(set.values()); // outputs [2]

set.delete(2);
console.log(set.values()); // outputs []

const setA = new Set();
setA.add(1);
setA.add(2);
setA.add(3);

const setB = new Set();
setB.add(2);
setB.add(3);
setB.add(4);

// --------- Union ----------
const union = (set1, set2) => {
  const unionAb = new Set();
  set1.forEach(value => unionAb.add(value));
  set2.forEach(value => unionAb.add(value));
  return unionAb;
};
console.log(union(setA, setB));

console.log(new Set([...setA, ...setB]));

// --------- Intersection ----------
const intersection = (set1, set2) => {
  const intersectionSet = new Set();
  set1.forEach(value => {
    if (set2.has(value)) {
      intersectionSet.add(value);
    }
  });
  return intersectionSet;
};
console.log(intersection(setA, setB));

console.log(new Set([...setA].filter(x => setB.has(x))));

// alternative - works on FF only
// console.log(new Set([x for (x of setA) if (setB.has(x))]));

// --------- Difference ----------
const difference = (set1, set2) => {
  const differenceSet = new Set();
  set1.forEach(value => {
    if (!set2.has(value)) {
      differenceSet.add(value);
    }
  });
  return differenceSet;
};
console.log(difference(setA, setB));

console.log(new Set([...setA].filter(x => !setB.has(x))));

// alternative  - works on FF only
// console.log(new Set([x for (x of setA) if (!setB.has(x))]));

```
```
// 注意上述内容在自创的set类中打印值为
const set = new Set()
set.add(1)
console.log(set.values()) // [1]
console.log(set.has(1)) // true
console.log(set.size) 
// ƒ size() {
//    return Object.keys(this.items).length
// }
```

> 使用扩展运算符,使得计算并集，交集，差集更简便（包含在上面的代码中，下面单独列出），包括以下三个步骤
1. 将集合转化为数组
2. 执行需要的运算
3. 结果转换为集合
```
// --------- Union ----------
console.log(new Set([...setA, ...setB]));

// --------- Intersection ----------
console.log(new Set([...setA].filter(x => setB.has(x))));

// --------- Difference ----------
console.log(new Set([...setA].filter(x => !setB.has(x))));
```
