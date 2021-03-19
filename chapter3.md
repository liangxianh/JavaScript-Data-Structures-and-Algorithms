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
3. 数组的entries keys values方法
4. form方法
5. Array.of方法
6. fill方法
7. copyWithin方法
8. 
