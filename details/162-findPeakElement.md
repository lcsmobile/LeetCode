162. 寻找峰值
162-findPeakElement
峰值元素是指其值大于左右相邻值的元素。

给定一个输入数组 nums，其中 nums[i] ≠ nums[i+1]，找到峰值元素并返回其索引。

数组可能包含多个峰值，在这种情况下，返回任何一个峰值所在位置即可。

你可以假设 nums[-1] = nums[n] = -∞。

示例 1:
```
输入: nums = [1,2,3,1]
输出: 2
解释: 3 是峰值元素，你的函数应该返回其索引 2。
```
示例 2:
```
输入: nums = [1,2,1,3,5,6,4]
输出: 1 或 5 
解释: 你的函数可以返回索引 1，其峰值元素为 2；
     或者返回索引 5， 其峰值元素为 6。
```
说明:

你的解法应该是 O(logN) 时间复杂度的。

```
语言:C
解题思路:二分法, 查找数组的中间一个值 mid,将其与前一个left和后一个值rigth比较
        情况1,left < mid < rigth  返回 mid 的索引
        情况2, mid < rigth 必然在右面有一个峰值 则从rigth 开始 进行二分 mid 的索引
        情况3, mid < left 必然在左面有一个左面有一个峰值 则从left 开始 进行二分 mid 的索引
        
        
int findPeakElement(int* nums, int numsSize) {
       if(numsSize < 3){
           if(numsSize == 1) return 0; 
           return nums[0] > nums[1]?0:1;
       }
       int start = 0, end=numsSize - 1;
       int mid = end/2;
        while (start < end) {
            mid = start + (end- start)/2;
            if (nums[mid] < nums[mid+1]){
                start=mid + 1; 
            } else if(nums[mid] < nums[mid-1]){
                end =mid - 1 ;
            }else{
                return mid;
            }
        }
    return end;
}
```
