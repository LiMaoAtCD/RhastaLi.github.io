---
layout: post
title: find max gif value
date: 2018-02-06 21:57 +0800
---

在一个m x n 的棋盘里面每一个格子放有一个礼物，礼物的价值大于0，你可以从棋盘的左上角开始礼物，每次向下或者向右移动一格，直到棋盘右下角，你如何规划路线使得礼物可以获得最大价值？

```
[1,2,3,5]
[3,4,5,6]
[7,7,5,4]
[0,2,4,6]
[3,9,3,2]
```




解法如下

```


var gift_matrix = [[1,2,3,5],[3,4,5,6],[7,7,5,4],[0,2,4,6],[3,9,3,2]]

func findMaxGiftIn(matrix: inout [[Int]]) -> Int {
    for i in 0..<matrix.count {
        for j in 0..<matrix[i].count {
            if i > 0 && j > 0 {
                matrix[i][j] = max(matrix[i - 1][j], matrix[i][j - 1]) + matrix[i][j]
            }
        }
    }

    let i = matrix.count - 1
    let j = matrix[0].count - 1
    return matrix[i][j]
}

findMaxGiftIn(matrix: &gift_matrix)

```

