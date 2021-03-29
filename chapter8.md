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

2. 散列表，hashTable类，hashMap类等是一种散列表的实现形式

> 以电子邮件地址簿为例，下面产检的散列函数，简单的将每个键值中每个字母的ASCII值相加

|  名称/键   | 散列函数  | 散列值 |  散列表  |
|   ----   |  ----   |  ----   |   ----   |
| Gandalf  | 71+97+110+100+97+108+102 | 685  | ... |
| John     | 74+111+104+110           | 399  | ... |
| Tyrion   | 84+121+114+105+111+110   | 645  | ... |

> 创建散列表
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
class HashTable {
  constructor(toStrFn = defaultToString) {
    this.toStrFn = toStrFn
    this.table = {}
  }
  // 创建散列函数
  loseloseHashCode(key) {
    if (typeof key === 'number') {
      return key
    }
    const tableKey = this.toStrFn(key)
    let hash = 0
    for (let i = 0; i < tableKey.length; i++) {
      hash += tableKey.charCodeAt(i)
    }
    // 利用hash % 数 为了规避操作数超过数值变量最大表示范围的风险
    return hash % 37
  }
  hashCode(key) {
    return this.loseloseHashCode(key)
  }
  // 将键和值加入散列表
  put(key, value) {
    if (key != null && value != null) {
      const position = this.hashCode(key)
      this.table[position] = new ValuePair(key, value)
      return true
    }
    return false
  }
  // 从散列表中获取一个值
  get(key) {
    const valuePair = this.table[this.hashCode(key)]
    // console.log('get---', this.table)
    return valuePair == null ? undefined : valuePair.value
  }
  /*
  HashTable类 与 Dictionary类很相似，不同之处在于
  Dictionary类中经valuePair保存在了的key属性中
  HashTable类中，有可以key（hash）生成一个数，并将valuePair保存在hash位置
  */
  // 从散列表中移除值
  remove(key){
    const hash = this.hashCode(key)
    const valuePair = this.table[hash]
    if (valuePair != null) {
      delete this.table[hash]
      return true
    }
    return false
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
    const keys = Object.keys(this.table)
    let objString = `${keys[0]} => ${this.table[keys[0]].toString()}`
    for(let i = 1; i < keys.length; i++) {
      objString = `${objString}, ${keys[i]} => ${this.table[keys[i]].toString()}`
    }
    return objString
  }
}
```
> 使用HashTable散列表
```
const hash = new HashTable();

console.log(hash.hashCode('Gandalf') + ' - Gandalf'); // 19 - Gandalf
console.log(hash.hashCode('John') + ' - John'); // 29 - John
console.log(hash.hashCode('Tyrion') + ' - Tyrion'); // 16 - Tyrion

console.log(' ');

console.log(hash.hashCode('Ygritte') + ' - Ygritte'); // 4 - Ygritte
console.log(hash.hashCode('Jonathan') + ' - Jonathan'); // 5 - Jonathan
console.log(hash.hashCode('Jamie') + ' - Jamie'); // 5 - Jamie
console.log(hash.hashCode('Jack') + ' - Jack'); // 7 - Jack
console.log(hash.hashCode('Jasmine') + ' - Jasmine'); // 8 - Jasmine
console.log(hash.hashCode('Jake') + ' - Jake'); // 9 - Jake
console.log(hash.hashCode('Nathan') + ' - Nathan'); // 10 - Nathan
console.log(hash.hashCode('Athelstan') + ' - Athelstan'); // 7 - Athelstan
console.log(hash.hashCode('Sue') + ' - Sue'); // 5 - Sue
console.log(hash.hashCode('Aethelwulf') + ' - Aethelwulf'); // 5 - Aethelwulf
console.log(hash.hashCode('Sargeras') + ' - Sargeras'); // 10 - Sargeras

hash.put('Ygritte', 'ygritte@email.com');
hash.put('Jonathan', 'jonathan@email.com');
hash.put('Jamie', 'jamie@email.com');
hash.put('Jack', 'jack@email.com');
hash.put('Jasmine', 'jasmine@email.com');
hash.put('Jake', 'jake@email.com');
hash.put('Nathan', 'nathan@email.com');
hash.put('Athelstan', 'athelstan@email.com');
hash.put('Sue', 'sue@email.com');
hash.put('Aethelwulf', 'aethelwulf@email.com');
hash.put('Sargeras', 'sargeras@email.com');

console.log('**** Printing Hash **** ');

console.log(hash.toString());
// {4 => [#Ygritte: ygritte@email.com]},{5 => [#Aethelwulf: aethelwulf@email.com]},{7 => [#Athelstan: athelstan@email.com]},{8 => [#Jasmine: jasmine@email.com]},{9 => [#Jake: jake@email.com]},{10 => [#Sargeras: sargeras@email.com]}

console.log('**** Get **** ');

console.log(hash.get('Ygritte')); // ygritte@email.com
console.log(hash.get('Loiane')); // jasmine@email.com

console.log('**** Remove **** ');

hash.remove('Ygritte');
console.log(hash.get('Ygritte')); // undefined

console.log(hash.toString());
// {5 => [#Aethelwulf: aethelwulf@email.com]},{7 => [#Athelstan: athelstan@email.com]},{8 => [#Jasmine: jasmine@email.com]},{9 => [#Jake: jake@email.com]},{10 => [#Sargeras: sargeras@email.com]}
```
> 散列集合：由一个集合构成，但是插入移除或者获取元素时，使用的是hashCode函数，不同之处在于不在添加键值对，而是只插入值没有键。eg 可以用散列结合来存储所有英文单词，此处不再实现；

> 解决冲突，在上面使用过程中，一些键有相同的散列值，导致有很多重复的keyhash值，之前添加的值都被覆盖，处理这种冲突有几种方法：分离链接，线性探查和双散列法

> 分离链接，该方法包括为散列表的每一个位置创建一个链接并将元素存储在里面，
```
// 创建分离链表
class HashTableSeparateChaining {
  constructor(toStrFn = defaultToString) {
    this.toStrFn = toStrFn
    this.table = {}
  }
  loseloseHashCode(key) {
    if (typeof key === 'number') {
      return key
    }
    const tableKey = this.toStrFn(key)
    let hash = 0
    for (let i = 0; i < tableKey.length; i++) {
      hash += tableKey.charCodeAt(i)
    }
    // 利用hash % 数 为了规避操作数超过数值变量最大表示范围的风险
    return hash % 37
  }
  hashCode(key) {
    return this.loseloseHashCode(key)
  }
  // 将键和值加入分离链表
  put(key, value) {
    if (key != null && value != null) {
      const position = this.hashCode(key)
      if (this.table[position] == null) {
        this.table[position] = new LinkedList()
      }
      this.table[position].push(new ValuePair(key, value))
      return true
    }
    return false
  }
  // 从分离链表中获取一个值
  get(key) {
    const position = this.hashCode(key)
    const linkedList = this.table[position]
    if (linkedList != null && !linkedList.isEmpty()) {
      let current = linkedList.getHead()
      while (current != null) {
        if (current.element.key === key) {
          return current.element.value
        }
        current = current.next
      }
    }
  }
  // 从分离链表中移除值
  remove(key){
    const position = this.hashCode(key)
    const linkedList = this.table[position]
    if (linkedList != null && !linkedList.isEmpty()) {
      let current = linkedList.getHead()
      while (current != null) {
        if (current.element.key === key) {
          linkedList.remove(current.element)
          if (linkedList.isEmpty()) {
            delete this.table[position]
          }
          return true
        }
        current = current.next
      }
    }
    return false
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
    const keys = Object.keys(this.table)
    let objString = `${keys[0]} => ${this.table[keys[0]].toString()}`
    for(let i = 1; i < keys.length; i++) {
      objString = `${objString}, ${keys[i]} => ${this.table[keys[i]].toString()}`
    }
    return objString
  }
}
const hash = new HashTableSeparateChaining();
console.log(' ');
console.log(hash.hashCode('Ygritte') + ' - Ygritte'); // 4 - Ygritte
console.log(hash.hashCode('Jonathan') + ' - Jonathan'); // 5 - Jonathan
console.log(hash.hashCode('Jamie') + ' - Jamie'); // 5 - Jamie
console.log(hash.hashCode('Jack') + ' - Jack'); // 7 - Jack
console.log(hash.hashCode('Jasmine') + ' - Jasmine'); // 8 - Jasmine
console.log(hash.hashCode('Jake') + ' - Jake'); // 9 - Jake
console.log(hash.hashCode('Nathan') + ' - Nathan'); // 10 - Nathan
console.log(hash.hashCode('Athelstan') + ' - Athelstan'); // 7 - Athelstan
console.log(hash.hashCode('Sue') + ' - Sue'); // 5 - Sue
console.log(hash.hashCode('Aethelwulf') + ' - Aethelwulf'); // 5 - Aethelwulf
console.log(hash.hashCode('Sargeras') + ' - Sargeras'); // 10 - Sargeras

hash.put('Ygritte', 'ygritte@email.com');
hash.put('Jonathan', 'jonathan@email.com');
hash.put('Jamie', 'jamie@email.com');
hash.put('Jack', 'jack@email.com');
hash.put('Jasmine', 'jasmine@email.com');
hash.put('Jake', 'jake@email.com');
hash.put('Nathan', 'nathan@email.com');
hash.put('Athelstan', 'athelstan@email.com');
hash.put('Sue', 'sue@email.com');
hash.put('Aethelwulf', 'aethelwulf@email.com');
hash.put('Sargeras', 'sargeras@email.com');

console.log('**** Printing Hash **** ');

console.log(hash.toString());
// 4 => [#Ygritte: ygritte@email.com], 5 => [#Jonathan: jonathan@email.com],[#Jamie: jamie@email.com],[#Sue: sue@email.com],[#Aethelwulf: aethelwulf@email.com], 7 => [#Jack: jack@email.com],[#Athelstan: athelstan@email.com], 8 => [#Jasmine: jasmine@email.com], 9 => [#Jake: jake@email.com], 10 => [#Nathan: nathan@email.com],[#Sargeras: sargeras@email.com]
console.log('**** Get **** ');

console.log(hash.get('Ygritte')); // ygritte@email.com
console.log(hash.get('Loiane')); // jasmine@email.com

console.log('**** Remove **** ');

hash.remove('Ygritte');
console.log(hash.get('Ygritte')); // undefined

console.log(hash.toString());
// 5 => [#Jonathan: jonathan@email.com],[#Jamie: jamie@email.com],[#Sue: sue@email.com],[#Aethelwulf: aethelwulf@email.com], 7 => [#Jack: jack@email.com],[#Athelstan: athelstan@email.com], 8 => [#Jasmine: jasmine@email.com], 9 => [#Jake: jake@email.com], 10 => [#Nathan: nathan@email.com],[#Sargeras: sargeras@email.com]

hash.remove('Sue')
console.log(hash.get('Sue')); // undefined
console.log(hash.toString());
// 5 => [#Jonathan: jonathan@email.com],[#Jamie: jamie@email.com],[#Aethelwulf: aethelwulf@email.com], 7 => [#Jack: jack@email.com],[#Athelstan: athelstan@email.com], 8 => [#Jasmine: jasmine@email.com], 9 => [#Jake: jake@email.com], 10 => [#Nathan: nathan@email.com],[#Sargeras: sargeras@email.com]
```
> 线性探索，当想在表中某个位置添加元素时，如果索引position的位置被占用了，就尝试position+1的位置，如果position+1的位置也被占了则尝试position+2，以此类推，因为会将key value都保存，查找时可以通过key的不同进行查看；注意单移除一个值时，有两种方式：1 软删除（时间积累会降低效）; 2 需要检验是否有必要将一个或多个元素移动到之前的位置
```
// 线性探索的put方法
  putl(key, value){
    if (key != null && value != null) {
      const position = this.hashCode(key)
      if (this.table[position] == null) {
        this.table[position] = new ValuePair(key, value)
      } else {
        let index = position + 1
        while (this.table[index] != null) {
          index++
        }
        this.table[index] = new ValuePair(key, value)
      }
      return true
    }
    return false
  }
  getl(key) {
    const position = this.hashCode(key)
    if (this.table[position] != null) {
      if (this.table[position].key === key) {
        return this.table[position].value
      } else {
        let index = position + 1
        while (this.table[index] != null && this.table[position].key != key) {
          index++
        }
        if (this.table[index] != null && this.table[index].key === key) {
          return this.table[index].value
        }
      }
    }
    return undefined
  }
  removel(key) {
    const position = this.hashCode(key)
    if (this.table[position] != null) {
      if (this.table[position].key === key) {
        delete this.table[position].value
        this.verifyRemoveSideEffect(key, position)
        return true
      }
      let index = position + 1
      while (this.table[index] != null && this.table[position].key != key) {
        index++
      }
      if (this.table[index] != null && this.table[index].key === key) {
        delete this.table[index].value
        this.verifyRemoveSideEffect(key, position)
        return true
      }
    }
    return false
  }
  // 由于不知道在散列表的不同位置上是否存在具有相同hash的元素，需要验证删除操作是否有副作用，如果有需要将冲突的元素移动到一个之前的位置
  verifyRemoveSideEffect(key, removedPosition) {
    const hash = this.hashCode(key)
    let index = removedPosition + 1
    while (this.table[index] != null) {
      const posHash = this.hashCode(this.table[index].key)
      if (posHash < hash || posHash <= removedPosition) {
        this.table[removedPosition] = this.table[index]
        delete this.table[index]
        removedPosition = index
      }
      index++
    }
  }
```
> 创建更好的散列函数，之前创建的散列函数 lose lose会产出太多的冲突,下面的方式可以很好的降低这种冲突
```
djb2HashCode(key) {
  const tableKey = this.toStrFn(key)
  // 5381这个质数，在测试中5381只是在测试中导致更少的碰撞和更好的雪崩数量。您几乎可以在每个哈希算法中找到“魔术常数”。
  let hash = 5381
  for (let i = 0; i< tableKey.length; i++) {
    hash = (hash * 33) + tableKey.charCodeAt(i)
  }
  return hash % 1013
}
console.log(hash.hashCode('Ygritte') + ' - Ygritte'); // 4 - Ygritte   ----> 807 - Ygritte
console.log(hash.hashCode('Jonathan') + ' - Jonathan'); // 5 - Jonathan   ----> 288 - Jonathan
console.log(hash.hashCode('Jamie') + ' - Jamie'); // 5 - Jamie   ----> 962 - Jamie
console.log(hash.hashCode('Jack') + ' - Jack'); // 7 - Jack   ----> 619 - Jack
console.log(hash.hashCode('Jasmine') + ' - Jasmine'); // 8 - Jasmine   ----> 275 - Jasmine
console.log(hash.hashCode('Jake') + ' - Jake'); // 9 - Jake   ----> 877 - Jake
console.log(hash.hashCode('Nathan') + ' - Nathan'); // 10 - Nathan   ----> 223 - Nathan
console.log(hash.hashCode('Athelstan') + ' - Athelstan'); // 7 - Athelstan   ----> 925 - Athelstan
console.log(hash.hashCode('Sue') + ' - Sue'); // 5 - Sue   ----> 502 - Sue
console.log(hash.hashCode('Aethelwulf') + ' - Aethelwulf'); // 5 - Aethelwulf   ----> 149 - Aethelwulf
console.log(hash.hashCode('Sargeras') + ' - Sargeras'); // 10 - Sargeras   ----> 711 - Sargeras
```

> ES2015 Map 类
```
const map = new Map();

map.set('Gandalf', 'gandalf@email.com');
map.set('John', 'johnsnow@email.com');
map.set('Tyrion', 'tyrion@email.com');

console.log(map.has('Gandalf')); // true
console.log(map.size); // 3

console.log(map.keys()); // MapIterator {"Gandalf", "John", "Tyrion"}
console.log(map.values()); // MapIterator {"gandalf@email.com", "johnsnow@email.com", "tyrion@email.com"}
console.log(map.get('Tyrion')); // tyrion@email.com

map.delete('John');

console.log(map.keys()); // MapIterator {"Gandalf", "Tyrion"}
console.log(map.values()); // MapIterator {"gandalf@email.com", "tyrion@email.com"}
```

> ES2015 WeakMap类和WeakSet类，分别是Map和Set两类的弱化版本，区别如下：

1. WeakMap类和WeakSet类没有entries，keys，values等方法，必须用键才可以取出值
2. 只能用对象做为键
```
const map = new WeakMap();

const ob1 = { name: 'Gandalf' };
const ob2 = { name: 'John' };
const ob3 = { name: 'Tyrion' };

map.set(ob1, 'gandalf@email.com');
map.set(ob2, 'johnsnow@email.com');
map.set(ob3, 'tyrion@email.com');

console.log(map.has(ob1)); // true
console.log(map.has(ob2)); // true
console.log(map.has(ob3)); // true

console.log(map.get(ob3)); // tyrion@email.com

map.delete(ob2);
console.log(map.has(ob2)); // false
----------------------------------
var set = new WeakSet();

const ob1 = { name: 'Gandalf' };
const ob2 = { name: 'John' };
const ob3 = { name: 'Tyrion' };

set.add(ob1);
set.add(ob2);
set.add(ob3);

console.log(set.has(ob1)); // true
console.log(set.has(ob2)); // true
console.log(set.has(ob3)); // true

set.delete(ob2);
console.log(set.has(ob2)); // false
```







