---
layout: post
title:  "复制链表"
date:   2018-02-02 21:28:12 +0800
---

```
import UIKit


class LinkedNode<T> {
    var value: T?
    
    var next: LinkedNode<T>?
    var sibling: LinkedNode<T>?
    init(value: T) {
        self.value = value
    }
    
    func insert(value: T) {
        let newNode = LinkedNode.init(value: value)
        var node = self
        while node.next != nil {
            node = node.next!
        }
        node.next = newNode
    }
    
}

class LinkedList<T>: CustomStringConvertible {
    

    var head: LinkedNode<T>?
    
    func insert(value: T) {
        if let node = self.head {
            node.insert(value: value)
        } else {
            self.head = LinkedNode.init(value: value)
        }
    }
    
    
    var description: String {
        var str = ""
        if var node = head {
            while node.next != nil {
                if let v = node.value {
                    str +=  "->\(v)"
                }
                node = node.next!
            }
            if let v = node.value {
                str += "->\(v)"
            }
        }
        return str
    }
    
    convenience init(arr: [T]) {
        self.init()
        guard !arr.isEmpty else { return }
        
        for value in arr {
           self.insert(value: value)
        }
        
        if let h = head {
            h.next
        }
    }
}



extension LinkedList {
    func copy(){
        
        guard var node = self.head else { return }
        
        while node.next != nil {
            let newNode = LinkedNode<T>.init(value: node.value!)
            newNode.next = node.next
            node.next = newNode
            node = newNode.next!
            newNode.sibling = node.sibling?.next
        }
        let newNode = LinkedNode<T>.init(value: node.value!)
        newNode.next = nil
        newNode.sibling = nil
        node.next = newNode


        head = self.head?.next
        guard var newIte = head else { return }
        while newIte.next?.next != nil {
            if let next = newIte.next?.next {
                newIte.next = next
                newIte = newIte.next!
            }
        }
        
        
    }
}
let list = LinkedList<Int>.init(arr: [1,2,3,4])

list.copy()

```