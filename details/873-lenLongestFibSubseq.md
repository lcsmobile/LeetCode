
873. 最长的斐波那契子序列的长度
如果序列 X_1, X_2, ..., X_n 满足下列条件，就说它是 斐波那契式 的：

n >= 3
对于所有 i + 2 <= n，都有 X_i + X_{i+1} = X_{i+2}
给定一个严格递增的正整数数组形成序列，找到 A 中最长的斐波那契式的子序列的长度。如果一个不存在，返回  0 。

（回想一下，子序列是从原序列 A 中派生出来的，它从 A 中删掉任意数量的元素（也可以不删），而不改变其余元素的顺序。例如， [3, 5, 8] 是 [3, 4, 5, 6, 7, 8] 的一个子序列）
```
示例 1：

输入: [1,2,3,4,5,6,7,8]
输出: 5
解释:
最长的斐波那契式子序列为：[1,2,3,5,8] 。
```
```
示例 2：

输入: [1,3,7,11,12,14,18]
输出: 3
解释:
最长的斐波那契式子序列有：
[1,11,12]，[3,11,14] 以及 [7,11,18] 。
```

提示：
```
3 <= A.length <= 1000
1 <= A[0] < A[1] < ... < A[A.length - 1] <= 10^9
（对于以 Java，C，C++，以及 C# 的提交，时间限制被减少了 50%）
```
```
语言:java
思路解析: n个数的数组中最长的子序列个数应该为 (1 .. n-1)这个数组中的最长序列个数 max或max+1, 以此类推那么(1..3)的数组中的子序列个数 依次累加 就能得的最大值 并且通过数组对每个序列的长度进行保存,最后回去最长的值

int lenLongestFibSubseq(int* A, int ASize) {
    long max = A[ASize - 1];
    int B[max] = @{0};
    for(int i = 0; i < ASize - 1;i++){
        //mp.instert(A[i],i);
        B[A[i]] = i;
    }
    int length = 0;
    int MaxLength = 0;
    for(int k = 0; k < ASize - 1 ;k++ ){
        for(int j = k ; j < ASize - 1;j++ ){
            int a = A[k] + A[j];
            while(B[a]){
                length++;
                a = a + A[B[a]];
            }
            if(length >= MaxLength){
              MaxLength =  length; 
            }
            length = 2;
        }
    }
    return MaxLength;
}
```
```
语言:swift
思路解析:同上
func lenLongestFibSubseq(_ A: [Int]) -> Int {
    var len = 0
    let count = A.count
    var dataDic: [Int: Int] = [:]
    for i in 0..<count {
    dataDic[A[i]] = i
    }

    var dataCount = [[Int]](repeating: [Int](repeating: 2, count: A.count), count: A.count)

    for i in stride(from:count - 1, through:0, by:-1) {
        for j in i+1..<count {
            let temp = A[i] + A[j]
            if let index = dataDic[temp] {
                dataCount[i][j] = dataCount[j][index] + 1
                len = max(len, dataCount[i][j])
             }
         }
    }
         
return len
}

```
