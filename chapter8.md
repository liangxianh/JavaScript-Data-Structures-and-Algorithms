## 字典和散列表

> 1 字典以[key, value]形式存储数据，又成映射，符号表或关联数组，字典经常在计算机科学中保存对象的引用地址
  > eg chrome|开发正工具，memory进行快走功能，我们就可以看到内存中的对象和他们对应的地址引用（用@<数>表示）

> 创建字典类及各种方法
```
function defaultToString(item) {
  if (item === null) {
    return 'NULL'
  } else if (item === undefined) {
    return 'UNDEFINED'
  } else if (typeof item === 'string' || item instanceof String) {
    return `${item}`
  }
  return item.toString()
}
class ValuePair {
  constructor(key, value) {
    this.key = key
    this.value = value
  }
  toString() {
    return `[#${this.key}: ${this.value}]`
  }
}
class Dictionary {
  constructor(toStrFn = defaultToString) {
    this.toStrFn = toStrFn
    // 会见[键, 值]对保存为 table[key] = [key, value]
    this.table = {}
  }
  // 检查一个键是否存在于字典中
  hasKey(key) {
    // 注意: js只允许我们使用字符串作为对象的键名或属性名，若传入复杂对象需要将其转化为字符串
    return this.table[this.toStrFn(key)] != null
  }
  // 在字典和ValuePair类中设置键和值
  set(key, value) {
    if (key != null && value != null) {
      const tableKey = this.toStrFn(key)
      this.table[tableKey] = new ValuePair(key, value)
      // return true 代表key value合法
      // console.log('set--', this.table)
      // { Gandalf: ValuePair {key: "Gandalf", value: "gandalf@email.com"}}
      return true
    }
    return false
  }
  // 在字典中移除一个值
  remove(key) {
    if (this.hasKey(key)) {
      delete this.table[this.toStrFn(key)]
      return true
    }
    return false
  }
  // 从字典中检索一个值
  get(key) {
    const valuePair = this.table[this.toStrFn(key)]
    return valuePair == null ? undefined : valuePair.value
  }
  keyValues() {
    return Object.values(this.table)
  }
  keys() {
    return this.keyValues().map(valuePair => valuePair.key)
  }
  values() {
    return this.keyValues().map(valuePair => valuePair.value)
  }
  forEach(fn) {
    const valuePairs = this.keyValues()
    for (let i = 0; i < valuePairs.length; i++) {
      const result = fn(valuePairs[i].key, valuePairs[i].value)
      if (result === false) {
        // ????为何fn返回false急中断for循环 用于fn报错处理进行中断处理么？？？？？？？？？？？？？？？？
        break;
      }
    }
  }
  size() {
    return Object.keys(this.table).length
  }
  isEmpty() {
    return this.size() === 0
  }
  clear() {
    this.table = {}
  }
  toString() {
    if (this.isEmpty()) {
      return ''
    }
    const valuePair = this.keyValues()
    let objString = `${valuePair[0].toString()}`
    for(let i = 1; i < valuePair.length; i++) {
      objString = `${objString}, ${valuePair[i].toString()}`
    }
    return objString
  }
}
```
> 使用Dictionary类
```
onst dictionary = new Dictionary();

dictionary.set('Gandalf', 'gandalf@email.com');
dictionary.set('John', 'johnsnow@email.com');
dictionary.set('Tyrion', 'tyrion@email.com');

console.log(dictionary.hasKey('Gandalf')); // true
console.log(dictionary.size()); // 3

console.log(dictionary.keys()); // ["Gandalf", "John", "Tyrion"]
console.log(dictionary.values()); // ["gandalf@email.com", "johnsnow@email.com", "tyrion@email.com"]
console.log(dictionary.get('Tyrion')); // tyrion@email.com

dictionary.remove('John');

console.log(dictionary.keys()); // ["Gandalf", "Tyrion"]
console.log(dictionary.values()); // ["gandalf@email.com", "tyrion@email.com"]

console.log(dictionary.keyValues()); // [{key: "Gandalf", value: "gandalf@email.com"}, {key: "Tyrion", value: "tyrion@email.com"}]
console.log(dictionary.toString()); // [#Gandalf: gandalf@email.com],[#Tyrion: tyrion@email.com]

dictionary.forEach((k, v) => {
  console.log('forEach: ', `key: ${k}, value: ${v}`);
});
// forEach:  key: Gandalf, value: gandalf@email.com
// forEach:  key: Tyrion, value: tyrion@email.com

```








