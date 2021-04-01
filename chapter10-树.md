## 树

> 创建树
```
function defaultCompare (e,n){
  return e === n ? 0 : e < n ? -1 : 1
}
// 创建Node节点
class Node {
  constructor(key) {
    this.key = key
    this.left = null
    this.right = null
  }
}
// 创建节点树
class BinarySearchTree {
  constructor(compareFn = defaultCompare) {
    this.compareFn = compareFn
    this.root = null
  }
  // 树的插入
  insert(key) {
    if (this.root == null) {
      this.root = new Node(key)
    } else {
      this.insertNode(this.root, key)
    }
  }
  insertNode(node, key) {
    if (this.compareFn(key, node.key) === -1) {
      // 小于节点值
      if (node.left == null) {
        node.left = new Node(key)
      } else {
        this.insertNode(node.left, key)
      }
    } else {
      if (node.right == null) {
        node.right = new Node(key)
      } else {
        this.insertNode(node.right, key)
      }
    }
  }
  // 树的遍历，分三种中序遍历^ 先序遍历|_  后序遍历_|
  /**
      2         1          3
    1   3      2 3        1  2
   */
  // 中序遍历,即从最小到最大的顺序访问所有节点，应用eg对树进行排序操作
  inOrderTraverse(callback) {
    this.inOrderTraverseNode(this.root, callback)
  }
  inOrderTraverseNode(node, callback) {
    if (node != null) {
      this.inOrderTraverseNode(node.left, callback)
      callback(node.key)
      this.inOrderTraverseNode(node.right, callback)
    }
  }
  // 先序遍历,先访问节点本身，在访问左侧节点，最后是右侧节点，eg打印一个结构化文档
  preOrderTraverse(callback) {
    this.preOrderTraverseNode(this.root, callback)
  }
  preOrderTraverseNode(node, callback) {
    if (node != null) {
      callback(node.key)
      this.preOrderTraverseNode(node.left, callback)
      this.preOrderTraverseNode(node.right, callback)
    }
  }
  // 后序遍历，先访问节点的后代节点，在访问节点本身，eg计算一个目录及其子目录中所有文件所占空间的大小
  postOrderTraverse(callback) {
    this.postOrderTraverseNode(this.root, callback)
  }
  postOrderTraverseNode(node, callback) {
    if (node != null) {
      this.postOrderTraverseNode(node.left, callback)
      this.postOrderTraverseNode(node.right, callback)
      callback(node.key)
    }
  }
  // 搜索树中的值，最小，最大，包含指定值 ,根据树的定义最小值即为最左最深节点的值，最大为最右最深的值
  min() {
    return this.minNode(this.root).key
  }
  minNode(node) {
    let current = node
    while(current != null && current.left != null) {
      current = current.left
    }
    return current
  }
  max() {
    return this.maxNode(this.root).key
  }
  maxNode(node) {
    let current = node
    while(current != null && current.right != null) {
      current = current.right
    }
    return current
  }
  has(key) {
    return this.hasNode(this.root, key)
  }
  hasNode(node, key) {
    if (node == null) {
      return false
    }
    if (this.compareFn(key, node.key) === -1) {
      return this.hasNode(node.left, key)
    } else if (this.compareFn(key, node.key) === 1){
      return this.hasNode(node.right, key)
    } else {
      return true
    }
  }
  // 移除节点，包含叶节点和非叶节点
  remove(key){
    this.root = this.removeNode(this.root, key)
  }
  removeNode(node, key) {
    if (node == null) {
      return null
    }
    if (this.compareFn(key, node.key) === -1){
      node.left = this.removeNode(node.left, key)
      return node
    } else if (this.compareFn(key, node.key) === 1) {
      node.right = this.removeNode(node.right, key)
      return node
    } else {
      // 键 = node.key
      // 第一种情况
      if(node.left == null && node.right == null) {
        node = null
        return node
      }
      // 第二种情况
      if(node.left == null) {
        node = node.right
        return node 
      } else if(node.right == null){
        node = node.left
        return node 
      }
      // 第三种情况
      const aux = this.minNode(node.right)
      node.key = aux.key
      node.right = this.removeNode(node.right, aux.key)
      return node
    }
  }
}
```

> 使用树
```

const tree = new BinarySearchTree()
tree.insert(11)
tree.insert(7)
tree.insert(15)
tree.insert(5)
tree.insert(3)
tree.insert(9)
tree.insert(8)
tree.insert(10)
tree.insert(13)
tree.insert(12)
tree.insert(14)
tree.insert(20)
tree.insert(18)
tree.insert(25)
tree.insert(6)
console.log(tree)

// 遍历
const printNode = (value) => console.log(value)
tree.inOrderTraverse(printNode) // 3 5 6 7 8 9 10 11 12 13 14 15 18 20 25
// tree.preOrderTraverse(printNode) // 11 7 5 3 6 9 8 10 15 13 12 14 20 18 25
// tree.postOrderTraverse(printNode) // 3 6 5 8 10 9 7 12 14 13 18 25 20 15 11
// 搜索
// console.log(tree.has(6))  // true
// console.log(tree.has(1))  // false
// console.log(tree.min())  // 3
// console.log(tree.max())  // 25

// 删除节点
tree.remove(6)
// tree.inOrderTraverse(printNode)
tree.remove(5)
```
> 自平衡树，上面创建的树没有进行控制，使得左右树的深度差值可能会很大，为了解决这个问题，引入了自平衡树AVL（Adelson-Velskii-Landi）
> AVL在添加或删除节点时会尝试保持自平衡,左子树和右子树高度最多相差1
