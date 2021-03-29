## 递归

> 递归，直接或间接的存在自身调用自身的方法或函数，一定注意需要一个基线条件即停止点eg，计算阶乘，可以用常规的迭代，也可以使用递归调用
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
  // console.trace()
  if (n === 1 || n === 0) {
    return 1;
  }
  return n * factorial(n - 1);
}

console.log('factorial(5): ', factorial(5)); // factorial(5):  120
console.log('factorial(3): ', factorial(3)); // factorial(3):  6
```

> js调用栈，每当一个函数或算法被调用时，该函数会进入调用栈的顶部，而递归调用因每次调用都可能依赖上一次调用的结果，故每个函数的调用会堆叠在调用栈的顶部；
> 可以打开浏览器在factorial 内部处断点查看call stack，发现在call stack处不断增加factorial调用，在return 1后不断减少

> js调用栈大小的限制，当递归被无限调用时浏览器会抛出错误，提示栈溢出，没个浏览器都有上限，可以用如下代码测试
```
let i = 0;
function recursiveFn() {
  i++;
  recursiveFn();
}

try {
  recursiveFn();
} catch (ex) {
  console.log('i = ' + i + ' error: ' + ex);
}
// chrome windows
//  i = 15733 error: RangeError: Maximum call stack size exceeded
// firforx windows
// i = 24971 error: InternalError: too much recursion
// 注意：根据操作系统和浏览器的不同，具体数据也会不同
```
> 实例，求斐波那契数
```
function fibonacci(n){
  if (n < 1) return 0; // {1}
  if (n <= 2) return 1; // {2}
  if (n === 3) {
    console.log('3 calculation nums') // 会被打印两次
  }
  return fibonacci(n - 1) + fibonacci(n - 2); // {3}
}

console.log('fibonacci(5)', fibonacci(5)); 
// 3 calculation nums
// 3 calculation nums
// fibonacci(5) 5
```
> 上面的内容会存在重复计算的情况，下面有一种记忆化的求斐波那契数方法
```
function fibonacciMemoization(n) {
  const memo = [0, 1];
  const fibonacci = (n) => {
    if (memo[n] != null) return memo[n];
    memo[n] = fibonacci(n - 1, memo) + fibonacci(n - 2, memo);
    console.log('memo', memo)
    return memo[n]
  };
  return fibonacci(n);
}

console.log('fibonacciMemoization(5)', fibonacciMemoization(5));
// memo (3) [0, 1, 1]
// memo (4) [0, 1, 1, 2]
// memo (5) [0, 1, 1, 2, 3]
// memo (6) [0, 1, 1, 2, 3, 5]
// fibonacciMemoization(5) 5
```

> 为何会使用递归，它更快么。实际多数情况迭代比递归会快，但是递归更好理解且使用性强，解决问题更简单






