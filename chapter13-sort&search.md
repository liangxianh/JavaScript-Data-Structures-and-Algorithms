### 排序和搜索算法

#### 排序算法

> 冒泡排序：比较相邻的两个项，如果第一个比第二个大则交换他们；复杂度O（n2）
```
function defaultCompare (e,n){
  return e === n ? 0 : e < n ? -1 : 1
}

function bubbleSort(array, compareFn = defaultCompare) {
  const { length } = array
  for (let i = 0; i < length; i++) {
    for (let j = 0; j < length - 1; j++) {
      console.log('bubbleSort1111111111111') //被打印了20次
      if (compareFn(array[j], array[j+1]) === 1) {
        swap(array, j, j+1)
      }
    }
  }
  return array;
}
/**
 * 这种情况经过第一轮循环，最大的数已经被筛到最后面，但是第二轮循环仍然还会对比，下面稍作修改
 */
function bubbleSort2(array, compareFn = defaultCompare) {
  const { length } = array
  for (let i = 0; i < length; i++) {
    for (let j = 0; j < length - i - 1; j++) {
      console.log('bubblesort222222222') //被打印了10次
      if (compareFn(array[j], array[j+1]) === 1) {
        swap(array, j, j+1)
      }
    }
  }
  return array;
}
function swap(array, a, b) {
  // const temp = array[a]
  // array[a] = array[b]
  // array[b] = temp
  [array[a], array[b]] = [array[b], array[a]]
}
function createNonSortedArray(){
  var array = [];
  for (let i = 5; i > 0; i--){
      array.push(i);
  }
  return array;
}

const array1 = bubbleSort(createNonSortedArray());
const array2 = bubbleSort2(createNonSortedArray());

console.log(array1, array2);
```

> 选择排序:找到最小值并将其放置在第一位，次小值放于第二位;复杂度O（n2）；
```
function SelectionSort(array, compareFn = defaultCompare) {
  const { length } = array
  let indexMin;
  for (let i = 0; i < length - 1; i++) {
    indexMin = i;
    for (let j = i; j < length; j++) {
      if (compareFn(array[indexMin], array[j]) === 1) {
        indexMin = j
      }
    }
    if (i != indexMin) {
      swap(array, i, indexMin)
    }
  }
  return array;
}
const array = SelectionSort(createNonSortedArray());
console.log(array);
```

> 插入排序:每次排一个数组项，,排序小型数组时，选择排序比冒泡和选择好些
```
function InsertSort(array, compareFn = defaultCompare) {
  const { length } = array
  let temp;
  for (let i = 0; i < length - 1; i++) {
    let j = i;
    temp = array[i]
    while(j > 0 && compareFn(array[j -1], temp) === 1) {
      array[j] = array[j - 1]
      j--
    }
    array[j] = temp
  }
  return array;
}
```
> 归并排序，一种分而治之的算法，较前3种性能更好，复杂度O(nlog(n))
```
function mergeSort(array, compareFn = defaultCompare) {
  if (array.length > 1) {
    const { length } = array
    const middle = Math.floor(length / 2)
    const left = mergeSort(array.slice(0, middle), compareFn)
    const right = mergeSort(array.slice(middle, length), compareFn)
    array = merge(left, right, compareFn)
  }
  return array
}
function merge(left, right, compareFn) {
  let i = 0;
  let j = 0;
  const result = []
  while (i < left.length && j < right.length) {
    result.push(compareFn(left[i], right[j]) === -1 ? left[i++] : right[j++])
  }
  return result.concat(i < left.length ? left.slice(i): right.slice(j))
}
```
> 快速排序，复杂度O(nlog(n))；
1. 在数据集之中，选择一个元素作为"基准"（pivot）。
2. 所有小于"基准"的元素，都移到"基准"的左边；所有大于"基准"的元素，都移到"基准"的右边。
3. 对"基准"左边和右边的两个子集，不断重复第一步和第二步，直到所有子集只剩下一个元素为止。
```
function quickSort(array, compareFn = defaultCompare){
  return quick(array, 0, array.length -1, compareFn)
}
function quick(array, left, right, compareFn){
  let index;
  if (array.length > 1) {
    index = partition(array, left, right, compareFn)
    console.log('array----', array)
    if (left < index -1) {
      quick(array, left, index - 1, compareFn)
    }
    if (index < right) {
      quick(array, index, right, compareFn)
    }
  }
  return array
}
// 主元随机选择数组中的一个值，一般选中间值
function partition(array, left, right, compareFn) {
  const pivot = array[Math.floor((left + right) / 2)]
  let i = left
  let j = right
  while (i <= j) {
    while (compareFn(array[i], pivot) === -1) {
      i++
    }
    while (compareFn(array[j], pivot) === 1) {
      j--
    }
    if (i <= j) {
      swap(array, i, j)
      i++
      j--
    }
  }
  return i
}
--------------------------------
上面的算法不需要新创建变量，下面的方法更加容易理解
var quickSort = function(arr) {
　　if (arr.length <= 1) { return arr; }
　　var pivotIndex = Math.floor(arr.length / 2);
　　var pivot = arr.splice(pivotIndex, 1)[0];
　　var left = [];
　　var right = [];
　　for (var i = 0; i < arr.length; i++){
　　　　if (arr[i] < pivot) {
　　　　　　left.push(arr[i]);
　　　　} else {
　　　　　　right.push(arr[i]);
　　　　}
　　}
　　return quickSort(left).concat([pivot], quickSort(right));
};
```

#### 搜索算法
```

```
