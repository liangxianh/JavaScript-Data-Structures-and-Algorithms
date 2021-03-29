## 递归

> 递归，直接或间接的存在自身调用自身的方法或函数，eg，计算阶乘，可以用常规的迭代，也可以使用递归调用
```
// 常规迭代
function factorialIterative(number) {
  if (number < 0) {
    return undefined;
  }
  let total = 1;
  for (let n = number; n > 1; n--) {
    total  = total * n;
  }
  return total;
}

console.log('factorialIterative(5): ', factorialIterative(5)); // factorialIterative(5):  120
console.log('factorialIterative(3): ', factorialIterative(3)); // factorialIterative(3):  6

// 递归方式
function factorial(n) {
  if (n === 1 || n === 0) {
    return 1;
  }
  return n * factorial(n - 1);
}

console.log('factorial(5): ', factorial(5)); // factorial(5):  120
console.log('factorial(3): ', factorial(3)); // factorial(3):  6
```
