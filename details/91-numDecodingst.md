
91. 解码方法
```
一条包含字母 A-Z 的消息通过以下方式进行了编码：

'A' -> 1
'B' -> 2
...
'Z' -> 26
给定一个只包含数字的非空字符串，请计算解码方法的总数。
```
```
示例 1:

输入: "12"
输出: 2
解释: 它可以解码为 "AB"（1 2）或者 "L"（12）。
示例 2:

输入: "226"
输出: 3
解释: 它可以解码为 "BZ" (2 26), "VF" (22 6), 或者 "BBF" (2 2 6) 。
```
方法一
swift 语言
思路:动态规划.记录0-n的情况 那么 0-(n+1) = (0-n) + x   ,x 由最后两位数字决定,依次类推,
x 如果最后两位组成的数值大于26 那么这种情况去掉,
如果连续出现两个0 则不能解码
新数不等于0
x = 上一个序列中最后一个尾数为个位数,且这个个位数和新添加的数值组成的数小于26的 情况.
新数等于0
那么原序列中最后一个尾数为2位数全部去掉 + 上一个序列中最后一个尾数为个位数,且这个个位数和新添加的数值组成的数小于26的 情况
```
class Solution {
   func numDecodings(_ s: String) -> Int {
        if s.count == 0{
            return 0
        }
        if s.count == 1 {
            
            return Int(s) == 0 ? 0 : 1
        }
        var strArr = [Int]()
        for  char :Character in s {
            let charStr = String(char)
            strArr.append(Int(charStr)!)
        }
        var arrCount : Int = 0
        var arrLastNum : Int = 0
        
        for  i in 1..<strArr.count {
                let k = i - 1
                let i_Int : Int = strArr[i]
                let k_Int : Int = strArr[k]
                let ik_Str = String(k_Int) + String(i_Int)
                let ik_Int : Int = Int(ik_Str)!
                if arrCount == 0 && ik_Int <= 26{
                    if k_Int == 0{
                       arrCount = 0
                       arrLastNum = 1
                        break
                    }else if i_Int == 0 {
                        arrCount = 1
                        arrLastNum = 0
                        
                    }else{
                        arrCount = 2
                        arrLastNum = 1
                    }
                }else if arrCount == 0 && ik_Int > 26 {
                    if k_Int == 0{
                        arrCount = 0
                        arrLastNum = 0
                        break
                    }else if i_Int == 0 {
                        arrCount = 0
                        arrLastNum = 0
                        break
                    }else{
                        arrCount = 1
                        arrLastNum = 1
                    }
                    
                }else if arrCount != 0 && ik_Int <= 26 {
                    let temp : Int = arrCount;
                    if i_Int != 0{
                        arrCount += arrLastNum
                        arrLastNum = temp
                    }else{
                        arrCount = arrLastNum
                        arrLastNum = 0
                    }
                    
                }else if arrCount != 0 && ik_Int > 26 {
                    
                    if i_Int != 0{
                       arrLastNum = arrCount
                    }else{
                        arrCount = 0
                        break
                    }
                }else{
                    arrCount = 0
                }
        }
        return   arrCount
    }
}
```
语言 jave
思路 :动态规划
```
class Solution {
    public int numDecodings(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        int[] nums = new int[s.length() + 1];
        nums[0] = 1;
        nums[1] = s.charAt(0) != '0' ? 1 : 0;
        for (int i = 2; i <= s.length(); i++) {
            if(s.charAt(i-1) != '0') {
                nums[i] = nums[i-1];
            }
            int twoDigits = (s.charAt(i - 2) - '0') * 10 + s.charAt(i - 1) - '0';
            if (twoDigits >= 10 && twoDigits <= 26) {
                nums[i] += nums[i - 2] ;
            }


        }
        return nums[s.length()] ;

    }
}
```
