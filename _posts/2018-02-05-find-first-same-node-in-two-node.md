---
layout: post
title:  "寻找两个链表的第一个相同节点"
date:   2018-02-02 21:28:12 +0800
---

```
class Node<T: Comparable> {
    var value: T
    var next: Node<T>?
    init(value: T) {
        self.value = value
    }
    
    func insert(_ v: T) {
        var node = self
        while node.next != nil {
            node = node.next!
        }
        let n = Node.init(value: v)
        node.next = n
    }

}

class LinkedList<T: Comparable> {
    var head: Node<T>?
    
    init(arr: [T]) {
        for v in arr {
            if self.head == nil {
                self.head = Node.init(value: v)
            } else {
                self.head?.insert(v)
            }
        }
    }
}



func getLengthOfLinkedList<T>(_ list: LinkedList<T>) -> Int {
    guard var node = list.head else { return 0 }
    var count = 0
    while node.next != nil {
        count += 1
        node = node.next!
    }
    return count
}

func findFirstCommonNodeInTwoLinkedList<T>( a: LinkedList<T>, b: LinkedList<T>) -> T? {
    let a_height = getLengthOfLinkedList(a)
    let b_height = getLengthOfLinkedList(b)
    var diff = a_height - b_height
    
    var node_a = a.head!
    var node_b = b.head!
    if diff > 0 {
        while diff != 0 {
            node_a = node_a.next!
            diff -= 1
        }
    } else {
        while diff != 0 {
            node_b = node_b.next!
            diff += 1
        }
    }
    
    while node_a.next != nil &&
        node_b.next != nil &&
        node_b.value != node_a.value {
        node_b = node_b.next!
        node_a = node_a.next!
    }
    
    
    return node_b.value
}


let a = LinkedList.init(arr: [0,1,2,3,4,5,6,7,8,9])
let b = LinkedList.init(arr: [10,4,5,6,7,8,9])

let result = findFirstCommonNodeInTwoLinkedList(a: a, b: b)
result
```



