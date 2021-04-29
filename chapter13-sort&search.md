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

```

#### 搜索算法
```

```
