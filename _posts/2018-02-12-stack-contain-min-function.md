---
layout: post
title: 包含min函数的栈
date: 2018-02-12 20:51 +0800
---

## 定义一个栈结构，实现一个min()可以得到栈的最小值，min() , pop(), push() 需要时间复杂度均为O(1)

```

class Stack<T: Comparable>: CustomStringConvertible {
    
    var bottom: Node<T>?
    var top: Node<T>?
    
    var description: String {
        guard var node = bottom else { return "empty" }
        var str = ""
        while node.next != nil {
            str += "\(node.value)->"
            node = node.next!
        }
        str += "\(node.value)"
        return str
    }
}

extension Stack {
    
    func push(value: T) {
        self.push(node: Node.init(value: value))
    }
    
    func push(node: Node<T>) {
        if self.top == nil {
            self.top = node
            self.top?.previous = nil
            self.bottom = node
        } else {
            self.top?.next = node
            node.previous = self.top
            self.top = node
        }
    }
    
    func pop() {
        if self.top == nil { return }
        self.top = self.top?.previous
        self.top?.next = nil
    }

}

class Node<T: Comparable> {
    var value: T
    init(value: T) {
        self.value = value
    }
    
    var next: Node<T>?
    weak var previous: Node<T>?
}


class SpecialStack<T: Comparable>: CustomStringConvertible {
    var description: String {
        return stack.description
    }
    let stack = Stack<T>.init()
    let help_stack = Stack<T>.init()
    func push(value: T) {
        stack.push(value: value)
        if let top = help_stack.top {
            help_stack.push(value: top.value > value ? value : top.value)
        } else {
            help_stack.push(value: value)
        }
    }
    
    func pop() {
        stack.pop()
        help_stack.pop()
    }
    
    func min() -> T? {
        return help_stack.top?.value
    }
}


// test case

let stack = SpecialStack<Int>.init()
stack.push(value: 2)
stack.push(value: 3)
stack.push(value: 4)
stack.push(value: 8)
stack.push(value: 10)
stack.push(value: 5)
stack.push(value: 9)
stack.push(value: 1)


stack.min()
stack.pop()
stack.min()




```