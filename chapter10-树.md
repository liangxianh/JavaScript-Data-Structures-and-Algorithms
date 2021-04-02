## 树

### 1 排序树
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

### 2 AVL树
> 自平衡树，上面创建的树没有进行控制，使得左右树的深度差值可能会很大，为了解决这个问题，引入了自平衡树AVL（Adelson-Velskii-Landi）
> AVL在添加或删除节点时会尝试保持自平衡,左子树和右子树高度最多相差1,下面进行创建自平衡树并添加方法
```
// 创建自平衡树,也是一种bst树，只有覆盖原有的insert insertNode 和removeNode即可
// 节点的高度和平衡因子
const BalanceFactor = {
    UNBALANCED_RIGHT: 1,
    SLIGHTLY_UNBALANCED_RIGHT: 2,
    BALANCED: 3,
    SLIGHTLY_UNBALANCED_LEFT: 4,
    UNBALANCED_LEFT: 5
}
class AVLTree extends BinarySearchTree {
  constructor(compareFn = defaultCompare) {
    super(compareFn)
    this.compareFn = compareFn
    this.root = null
  }
  getNodeHeight(node) {
    if (node == null) {
      return -1
    }
    const max = Math.max(this.getNodeHeight(node.left), this.getNodeHeight(node.right))
    // console.log(max)
    return max + 1
  }
  
  // 计算节点的平衡因子
  getBalanceFactor(node) {
    const heightDifference = this.getNodeHeight(node.left) - this.getNodeHeight(node.right)
    switch(heightDifference) {
      case -2:
        return BalanceFactor.UNBALANCED_RIGHT;
      case -1:
        return BalanceFactor.SLIGHTLY_UNBALANCED_RIGHT;
      case 1:
        return BalanceFactor.SLIGHTLY_UNBALANCED_LEFT;
      case 2:
        return BalanceFactor.UNBALANCED_LEFT;
      default:
        return BalanceFactor.BALANCED;
    }
  }
  /*
  进行平衡操作
  左左（LL）：单纯的向右旋转
  右右（RR）：单纯的向左旋转
  左右（LR）：向右的双旋转（先LL，再RR）
  右左（RL）：向左的双旋转（先RR，在LL）
  */
  // 左左操作 向右单旋转:左侧子节点高于右侧且左侧子节点也是平衡的或者左侧较重
  rotationLL(node) {
    const temp = node.left
    node.left = temp.right
    temp.right = node;
    return temp
  }
  // 右右 单纯的左旋转：右侧子节点高度大于左侧子节点高度且右侧的子节点也是平衡或者右侧较重
  rotationRR(node) {
    const temp = node.right
    node.right = temp.left
    temp.left = node
    return temp
  }
  // 左右：向右双旋转先LL在RR：左侧子节点高度大于右侧，并且左侧子节点右侧较重
  // 先LL在RR
  rotationLR(node) {
    node.left = this.rotationRR(node.left)
    return this.rotationLL(node)
  }
  // 右左：向左双旋转先RR在LL
  rotationRL(node) {
    node.right = this.rotationLL(node.right)
    return this.rotationRR(node)
  }
  // 插入节点
  insertA(key) {
    this.root = this.insertNodeA(this.root, key)
  }
  insertNodeA(node, key) {
    if (node == null) {
      return new Node(key)
    } else if (this.compareFn(key, node.key) === -1){
      node.left = this.insertNodeA(node.left, key)
    }else if (this.compareFn(key, node.key) === 1){
      node.right = this.insertNodeA(node.right, key)
    } else {
      return node //重复的键值
    }
    const balanceFactor = this.getBalanceFactor(node)
    // leftH - rightH = 2  即左侧高
    if (balanceFactor === BalanceFactor.UNBALANCED_LEFT) {
      if (this.compareFn(key, node.left.key) === -1) {
        return this.rotationLL(node)
      } else {
        return this.rotationLR(node)
      }
    }
    // leftH - rightH = -2  即右侧高
    if (balanceFactor === BalanceFactor.UNBALANCED_RIGHT) {
      if (this.compareFn(key, node.right.key) === 1) {
        return this.rotationRR(node)
      } else {
        return this.rotationRL(node)
      }
    }
    return node
  }
  // 在AVL中移除节点
  removeNode(node, key) {
    node = super.removeNode(node, key)
    if (node == null) {
      return null
    }
    // 检测数是否平衡
    const balanceFactor = this.getBalanceFactor(node)
    // leftH - rightH = 2  即左侧高
    if (balanceFactor === BalanceFactor.UNBALANCED_LEFT) {
      const balanceFactorLeft = this.getBalanceFactor(node.left)
      // left 平衡因子为默认值的 || leftH-rightH = 1
      if (balanceFactorLeft === BalanceFactor.BALANCED ||
        balanceFactorLeft === BalanceFactor.SLIGHTLY_UNBALANCED_LEFT) {
          return this.rotationLL(node)
        }
      // leftH-rightH = -1
      if(balanceFactorLeft === BalanceFactor.SLIGHTLY_UNBALANCED_RIGHT) {
        return this.rotationLR(node)
      }
      if (this.compareFn(key, node.left.key) === -1) {
        return this.rotationLL(node)
      } else {
        return this.rotationLR(node.left)
      }
    }
    // leftH - rightH = -2  即右侧高
    if (balanceFactor === BalanceFactor.UNBALANCED_RIGHT) {
      const balanceFactorRight = this.getBalanceFactor(node.right)
      // left 平衡因子为默认值的 || leftH-rightH = -1
      if (balanceFactorRight === BalanceFactor.BALANCED ||
        balanceFactorRight === BalanceFactor.SLIGHTLY_UNBALANCED_RIGHT) {
          return this.rotationRR(node)
        }
      // leftH-rightH = 1
      if(balanceFactorRight === BalanceFactor.SLIGHTLY_UNBALANCED_LEFT) {
        return this.rotationRL(node.right)
      }
    }
    return node
  }
}
```

> AVL树的使用
```
const avlTree = new AVLTree()
avlTree.insertA(70)
avlTree.insertA(50)
avlTree.insertA(80)
avlTree.insertA(72)
avlTree.insertA(90)

avlTree.insertA(75)
avlTree.insertA(77)
avlTree.insertA(78)
console.log(avlTree)
// 删除有点疑问？删除后怎么进行排序的
avlTree.remove(80)
console.log(avlTree)
```

### 3 红黑树
> 同AVL相同，红黑树也是一个自平衡二叉树，因AVL树插入和移除节点会造成旋转，顾我们需要一个包含多次插入和删除的自平衡树，红黑树是比较好的；当删除和插入频率较低avl树较好；
> 


