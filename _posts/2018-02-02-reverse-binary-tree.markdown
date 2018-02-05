---
layout: post
title:  "反转二叉树"
date:   2018-02-02 21:28:12 +0800
---

```
class TreeNode<T: Comparable> {
    var value: T
    var leftChild: TreeNode<T>?
    var rightChild: TreeNode<T>?
    
    init(value: T) {
        self.value = value
    }

}
extension TreeNode {
    
    func insert(_ value: T) {
        if self.value < value {
            if let rightChild = self.rightChild {
                rightChild.insert(value)
            } else {
                let node = TreeNode.init(value: value)
                self.rightChild = node
            }
        } else if self.value > value {
            if let left = self.leftChild {
                left.insert(value)
            } else {
                let node = TreeNode.init(value: value)
                self.leftChild = node
            }
        } else {
            print("same value in the tree")
        }
    }
    
    func reversed() {
        if let left = leftChild {
            left.reversed()
        }
        
        if let right = rightChild {
            right.reversed()
        }
        
        let temp = self.leftChild
        self.leftChild = self.rightChild
        self.rightChild = temp
    }
}

extension TreeNode: CustomStringConvertible {
    var description: String {
        var str = ""
        
        if let left = self.leftChild  {
            str += "[" + left.description + "]<-"
        }
        str += "\(self.value)"
        if let right = self.rightChild  {
            str += "[->" + right.description + "]"
        }
        
        return str
    }
}



let tree = TreeNode.init(value: 5)
tree.insert(1)
tree.insert(2)
tree.insert(3)
tree.insert(4)
tree.insert(5)
tree.insert(6)
tree.insert(7)
tree.insert(8)
tree.insert(9)
tree.insert(10)
tree.reversed()
print(tree)

```
