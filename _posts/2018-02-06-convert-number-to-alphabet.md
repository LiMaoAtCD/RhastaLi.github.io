---
layout: post
title: 将数字转换成字符串
date: 2018-02-06 22:38 +0800
---


0 -> a
1 -> b
.
.
.
25 -> z

定义一个函数得到一串数字转换成字符串的总数

这里没有处理Int超出的逻辑

```
extension Int {
    var numbers: [Int] {
        var number = self
        var numArr = [Int]()
        while number != 0 {
            let a = number % 10
            number = number / 10
            numArr.append(a)
        }
        return numArr.reversed()
    }
}

func getMaxCountFrom(number: Int) -> Int {
    var arr = number.numbers
    guard arr.count > 0 else { return 0 }
    
    var count = 1
    for i in 1..<arr.count {
        count += 1
        if (arr[i - i] * 10 + arr[i]) <= 25 {
            count += 1
        }
    }
    return count
}

```