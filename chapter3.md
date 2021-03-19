## 数组

### 数组方法
1. map 对数组中的每个元素进行给定函数，返回每次函数调用的结果组成的数组
2. filter对数组中的每个元素进行给定函数，返回该函数会返回true的元素组成的数组
3. reduce（previousValue,currentValue[,index][,array]）
```
如，求和
numbers.reduce((previous, current) => previous + curent)
```
4. concat 数组合并，concat方法可以向数组传递数组对象或是元素
```
const zero = 0;
const positiveNumbers = [1, 2, 3];
const negativeNumbers = [-3, -2, -1];
let numbers = negativeNumbers.concat(zero, positiveNumbers);
const curobj = { value: 1 }
let concatObj = negativeNumbers.concat(curobj, positiveNumbers);
console.log('negativeNumbers.concat(zero, positiveNumbers)', numbers);
console.log('negativeNumbers.concat(obj, positiveNumbers)', concatObj);
// 05-ArrayMethods.js:18 negativeNumbers.concat(zero, positiveNumbers) (7) [-3, -2, -1, 0, 1, 2, 3]
// 05-ArrayMethods.js:19 negativeNumbers.concat(obj, positiveNumbers) (7) [-3, -2, -1, {value：1}, 1, 2, 3]
```

### ECMAScript6和数组新功能
1. for...of
```
//* ********* using for..of loop
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15];
for (const n of numbers) {
  console.log(`for..of loop: ${n} % 2 === 0`, n % 2 === 0 ? 'even' : 'odd');
}
for (let k of numbers) {
  console.log('k', k)
}
```

2. 使用@@iterator对象，es2015新增，需要通过Symbol.iterator来访问
```
console.log('Using the new ES6 iterator (@@iterator)');
const numbers = [1, 2, 3, 4];
let iterator = numbers[Symbol.iterator]();
console.log('iterator', iterator); //iterator Array Iterator {}
console.log('iterator.next().value', iterator.next().value); // 1
console.log('iterator.next().value', iterator.next().value); // 2
console.log('iterator.next().value', iterator.next().value); // 3
console.log('iterator.next().value', iterator.next().value); // 4
console.log('iterator.next().value', iterator.next().value); // undefined
```
```
let iterator = numbers[Symbol.iterator](); //iterator Array Iterator {}
console.log('iterator.next().value', iterator.next().value); // 1
for (const n of iterator) {
  console.log(`${n} of iterator`, n);
}
打印结果，及next后的数据在iterator中不在存在
// iterator.next().value 1
// 2 of iterator 2
// 3 of iterator 3
// 4 of iterator 4
可以直接用for...of 或数组元素较少的情况直接是有next，
```

3. 数组的entries keys values方法
> entries方法返回包含键值对的@@iterator
```
let aEntries = numbers.entries(); // retrieve iterator of key/value
console.log('aEntries.next().value', aEntries.next().value); // [0, 1] - position 0, value 1
console.log('aEntries.next().value', aEntries.next().value); // [1, 2] - position 1, value 2
console.log('aEntries.next().value', aEntries.next().value); // [2, 3] - position 2, value 3
console.log('aEntries.next().value', aEntries.next().value); // [3, 4]
console.log('aEntries.next().value', aEntries.next().value); //undefined
// or use code below
aEntries = numbers.entries();
for (const n of aEntries) {
  console.log(`entry of ${n}`, n);
}
``` 
> keys方法返回包含数组索引的@@iterator，keys方法返回numbers数组的索引，一旦没有可迭代的值，aKeys.next()就会返回一个value属性为undefined、done属性为true的对象，done为false说明仍然可迭代
```
const aKeys = numbers.keys(); // retrieve iterator of keys
console.log('aKeys.next()', aKeys.next()); // {value: 0, done: false } done false means iterator has more values
console.log('aKeys.next()', aKeys.next()); // {value: 1, done: false }
console.log('aKeys.next()', aKeys.next()); // {value: 2, done: false }
console.log('aKeys.next()', aKeys.next()); // {value: 3, done: false}
console.log('aKeys.next()', aKeys.next()); // {value: undefined, done: true}
```
> values方法返回的@@iterator则包含数组的值 （有兼容性问题）
```
const aValues = numbers.values();
console.log(aValues.next()); // {value: 1, done: false } done false means iterator has more values
console.log(aValues.next()); // {value: 2, done: false }
console.log(aValues.next()); // {value: 3, done: false }
console.log(aValues.next()); // {value: 4, done: false}
console.log(aValues.next()); // {value: undefined, done: true}
```

4. form, Array.form方法根据已有的数组创建一个新数组。比如赋值numbers, <b>新建的数组属于深拷贝</b>
```
let numbersCopy1 = Array.from(numbers)
console.log('numbersCopy1', numbersCopy1) // numbersCopy1 (4) [1, 2, 3, 4]
numbersCopy1[1] = 90
console.log('numbersCopy1=', numbersCopy1, 'numbers=', numbers) // numbersCopy1= (4) [1, 90, 3, 4] numbers= (4) [1, 2, 3, 4]

const evens = Array.from(numbers, x => x % 2 === 0);
console.log('Array.from(numbers, x => x % 2 === 0)', evens); // Array.from(numbers, x => x % 2 === 0) (4) [false, true, false, true]
```

5. Array.of方法根据传入的参数创建一个新数组，类似array.form
```
const numbers3 = Array.of(1);
const numbers4 = Array.of(1, 2, 3, 4, 5, 6);
// 同下面效果一样
const numbers3 = [1]
const numbers4 = [1, 2, 3, 4, 5, 6]
// 也可以用下面的方法复制已有数组
const numbersCopy = Array.of(...numbers4);
```

6. fill方法用静态值填充数组，fill(value, startIndex, lastIndex),包含startIndex，不包含lastIndex
```
const numbers4 = Array.of(1, 2, 3, 4, 5, 6);
numbersCopy.fill(0);
console.log('numbersCopy.fill(0)', numbersCopy); // (6) [0, 0, 0, 0, 0, 0]

numbersCopy.fill(2, 1);
console.log('numbersCopy.fill(2, 1)', numbersCopy); // (6) [0, 2, 2, 2, 2, 2]

numbersCopy.fill(1, 3, 5);
console.log('numbersCopy.fill(1, 3, 5)', numbersCopy); // (6) [0, 2, 2, 1, 1, 2]

const ones = Array(6).fill(1);
console.log('Array(6).fill(1)', ones); // (6) [1, 1, 1, 1, 1, 1]
```

7. copyWithin方法复制数组中的一系列元素到同一数组指定的起始位置，copyWithin(需要被替换开始的index, 想要复制的元素开始的index[,想要复制的元素的结束index])
```
let copyArray = [1, 2, 3, 4, 5, 6];
console.log('copyArray', copyArray); // copyArray (6) [1, 2, 3, 4, 5, 6]

copyArray = copyArray.copyWithin(0, 3); // pos 3 value is copied to pos 0
console.log('copyArray.copyWithin(0, 3)', copyArray); // copyArray.copyWithin(0, 3) (6) [4, 5, 6, 4, 5, 6]

copyArray = [1, 2, 3, 4, 5, 6];
copyArray = copyArray.copyWithin(1, 3, 5); // pos 3-4 values are copied to pos 1-2
console.log('copyArray.copyWithin(1, 3, 5)', copyArray); // copyArray.copyWithin(1, 3, 5) (6) [1, 4, 5, 4, 5, 6]

copyArray = [1, 2, 3, 4, 5, 6];
copyArray = copyArray.copyWithin(2, 3, 5); // pos 3-4 values are copied to pos 1-2
console.log('copyArray.copyWithin(2, 3, 5)', copyArray); // copyArray.copyWithin(2, 3, 5) (6) [1, 2, 4, 5, 5, 6]
```

