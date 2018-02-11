---
layout: post
title: Binary Search Tree
date: 2018-02-11 15:45 +0800
---

```
class TreeNode<T: Comparable & CustomStringConvertible> {
    var value: T
    weak var parent: TreeNode<T>?
    var leftChild: TreeNode<T>?
    var rightChild: TreeNode<T>?
    var weight: Int = 0
    init(value: T) {
        self.value = value
    }
    
    var isParentLeftChild: Bool {
        if let parent_left = self.parent?.leftChild, parent_left == self {
            return true
        }
        return false
    }
    
    var isParentRightChild: Bool {
        if let parent_right = self.parent?.rightChild, parent_right == self {
            return true
        }
        return false
    }
    
    var minRightNode: TreeNode<T>? {
        guard var right = self.rightChild else { return nil}
        while right.leftChild != nil {
            right = right.leftChild!
        }
        return right
    }
    
    var maxLeftNode: TreeNode<T>? {
        guard var left = self.leftChild else { return nil}
        while left.rightChild != nil {
            left = left.rightChild!
        }
        return left
    }


}
extension TreeNode: Equatable, CustomStringConvertible {
    
    public static func ==(lhs: TreeNode, rhs: TreeNode) -> Bool {
        return lhs.value == rhs.value
    }
    
    var description: String {
        var str = ""
        if let left = self.leftChild {
            str += "[\(left.description)] <-"
        }
        str += "\(self.value)"
        if let right = self.rightChild {
            str += "-> [\(right.description)]"
        }
        return str
    }
}

extension TreeNode {
    func upStraightWeight() {
        var node = self
        while node.parent != nil {
            if self.isParentLeftChild {
                node.parent!.weight += 1
                if node.parent!.weight > 1 {
                    node.parent!.balance()
                    break
                }

                node = node.parent!
            } else {
                node.parent!.weight -= 1
                if node.parent!.weight < -1 {
                    node.parent!.balance()
                    break
                }
            }
        }
    }
    
    func balance() {
        if self.weight > 1 {
            let pivot = self.leftChild!

            if pivot.weight > 0 {

                //右旋
                self.leftChild = pivot.rightChild
                pivot.rightChild?.parent = self

                if self.isParentLeftChild {
                    self.parent?.leftChild = pivot
                } else {
                    self.parent?.rightChild = pivot
                }

                pivot.parent = self.parent

                pivot.rightChild = self
                self.parent = pivot
                self.weight -= 1
                pivot.weight -= 1

            } else {
                // 先左旋
                let new_pivot = pivot.rightChild
                new_pivot?.parent = self
                self.leftChild = new_pivot
                
                new_pivot?.leftChild = pivot
                pivot.parent = new_pivot
                pivot.rightChild = new_pivot?.leftChild
                new_pivot?.leftChild?.parent = pivot
                
                new_pivot?.weight += 1
                pivot.weight -= 1
//              再右旋
                self.balance()
            }
        }
        
        if self.weight < -1 {
            let pivot = self.rightChild!
            if pivot.weight < 0 {
                //左旋
                self.rightChild = pivot.leftChild
                pivot.leftChild?.parent = self
                if self.isParentLeftChild {
                    self.parent?.leftChild = pivot
                } else {
                    self.parent?.rightChild = pivot
                }
                pivot.parent = self.parent
                
                pivot.leftChild = self
                self.parent = pivot
                self.weight += 1
                pivot.weight += 1
            } else {
                // 先右旋
                let new_pivot = pivot.leftChild
                new_pivot?.parent = self
                self.rightChild = new_pivot
                
                new_pivot?.rightChild = pivot
                pivot.parent = new_pivot
                pivot.leftChild = new_pivot?.rightChild
                new_pivot?.rightChild?.parent = pivot
                
                new_pivot?.weight += 1
                pivot.weight -= 1
                // 再左旋
                self.balance()
            }
        }
    }
}

extension TreeNode {
    
    func delete(value: T) {
        
        if value < self.value {
            if let left = self.leftChild {
                left.delete(value: value)
            } else {
                print("没有可删除的节点")
            }
        } else if value > self.value {
            if let right = self.rightChild {
                right.delete(value: value)
            } else {
                print("没有可删除的节点")
            }
        } else {
            //删除操作
            
            // 分为4种情况 1，无子节点 2 单子节点
            
            if leftChild == nil {
                if self.isParentLeftChild {
                    self.parent?.leftChild = self.rightChild
                } else {
                    self.parent?.rightChild = self.rightChild
                }
            }
            
            if rightChild == nil {
                if self.isParentLeftChild {
                    self.parent?.leftChild = self.leftChild
                } else {
                    self.parent?.rightChild = self.leftChild
                }
            }
            
//            3 双子节点
            if let maxLeft = self.maxLeftNode {
                maxLeft.leftChild?.parent = maxLeft.parent
                maxLeft.parent?.rightChild = maxLeft.leftChild
                maxLeft.leftChild = self.leftChild
                maxLeft.rightChild = self.rightChild
                
                if self.isParentLeftChild {
                    self.parent?.leftChild = maxLeft
                } else {
                    self.parent?.rightChild = maxLeft
                }
            }
            
            
            
        }
    }
    
}

class BinarySearchTree<T: Comparable & CustomStringConvertible>: CustomStringConvertible {
    var root: TreeNode<T>?
    
    public func insert(value: T) {
        if let node = root {
//            root.insert(value: value)
            self.insertTo(node: node, value: value)
            
        } else {
            root = TreeNode.init(value: value)
        }
    }
    
    private func insertTo(node: TreeNode<T>, value: T) {
        if value < node.value {
            if let left = node.leftChild {
                insertTo(node: left, value: value)
            } else {
                let new = TreeNode.init(value: value)
                new.parent = node
                node.leftChild = new
            }
        } else if value > node.value {
            
            if let right = node.rightChild {
                insertTo(node: right, value: value)
            } else {
                let new = TreeNode.init(value: value)
                new.parent = node
                node.rightChild = new
            }
        } else {
            //
            print("已经存在")
        }
    }
    
    func search(value: T) -> TreeNode<T>? {
        if let node = self.root {
            return search(node: node, value: value)
        } else {
            print("null")
        }
        return nil
    }
    
    private func search(node: TreeNode<T>?, value: T) -> TreeNode<T>? {
        if node == nil  { return nil }
        if node!.value == value {
            return node
        } else if node!.value > value {
            return search(node: node!.leftChild, value: value)
        } else {
            return search(node: node!.rightChild, value: value)
        }
    }
    
    func delete(value: T) {
        if let node = search(value: value) {
            
            if node.leftChild == nil {
                var replace = node.rightChild
                
                if node.isParentLeftChild {
                    node.parent?.leftChild = replace
                } else if node.isParentRightChild {
                    node.parent?.rightChild = replace
                }
                replace?.parent = node.parent
                
                while replace?.parent != nil {
                    replace = replace?.parent
                }
                root = replace
            }
            
            if node.rightChild == nil {
                var replace = node.leftChild
                if node.isParentLeftChild {
                    node.parent?.leftChild = replace
                } else if node.isParentRightChild {
                    node.parent?.rightChild = replace
                }
                replace?.parent = node.parent
                while  replace?.parent != nil {
                    replace = replace?.parent
                }
                root = replace
            }
            
            if node.leftChild != nil && node.rightChild != nil {
                
                var replace = node.leftChild
                while replace?.rightChild != nil {
                    replace = replace?.rightChild
                }
                if replace == node.leftChild {
                    replace?.parent = node.parent
                    if node.isParentLeftChild {
                        node.parent?.leftChild = replace
                    } else if node.isParentRightChild {
                        node.parent?.rightChild = replace
                    }
                    replace?.rightChild = node.rightChild

                    
                    
                    while replace?.parent != nil {
                        replace = replace?.parent
                    }
                    root = replace
                } else {
                    let replace_leftChild = replace?.leftChild
                    replace_leftChild?.parent = node.leftChild
                    node.leftChild?.rightChild = replace_leftChild
                    
                    replace?.leftChild = node.leftChild
                    node.leftChild?.parent = replace
                    
                    replace?.rightChild = node.rightChild
                    node.rightChild?.parent = replace
                    
                    if node.isParentRightChild {
                        node.parent?.rightChild = replace
                    } else if node.isParentLeftChild {
                        node.parent?.leftChild = replace
                    }
                    
                    while replace?.parent != nil {
                        replace = replace?.parent
                    }
                    root = replace
                }
            }
        } else {
            print("不存在的节点")
        }
    }

    
    var description: String {
        guard let node = root else { return "empty"}
        return node.description
    }
}

```