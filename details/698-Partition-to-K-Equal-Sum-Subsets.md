Given an array of integers nums and a positive integer k, find whether it's possible to divide this array into k non-empty subsets whose sums are all equal.

**Example 1:**
> **Input:** nums = [4, 3, 2, 3, 5, 2, 1], k = 4  
**Output:** True  
**Explanation:** It's possible to divide it into 4 subsets (5), (1, 4), (2,3), (2,3) with equal sums.

**Note:**
- 1 <= k <= len(nums) <= 16.
- 0 < nums[i] < 10000.

## Java 代码

**思路：** 

1. 先求得分 k 组后每个子集的元素和为 avg
2. 从第一个子集开始划分，直到划分出满足要求的 k 个子集即返回 true
3. 遍历数组，向子集中添加元素，添加时使用 curSum 来记录当前自己中的元素和，通过 flags 数组来标记数组中对应索引处的元素是否已经被添加到子集中
4. 如果遍历到的元素值与 curSum 相加超过 avg，则该元素不能添加到该子集，继续遍历下一为止元素
5. 如果遍历到的元素值与 curSum 相加不超过 avg，则该元素能添加到该子集，将该元素添加到子集中后，继续遍历数组中下一元素
6. 如果遍历完集合后发现当前子集中元素和不等于 avg，则将该子集中最后一个元素移除，从集合中被移除的元素的下一个位置元素开始继续遍历
7. 找到一个子集后会继续从集合中未使用的元素中划分下一个子集，如果该子集构建成功即构建下一子集
8. 如果子集构建失败，则会返回上一子集的构建过程，将该子集中最后一个元素移除，从集合中被移除的元素的下一个位置元素开始继续遍历
9. 直到 k 个子集且数组中元素也全部被使用，即返回 true
10. 如果尝试所有组合后都无法满足条件，则返回 false


```
class Solution {
    public boolean canPartitionKSubsets(int[] nums, int k) {
        if (k == 1) { // 1 个子集，直接返回 true
			return true;
		}
        
        int sum = 0;
        for (int num : nums){
            sum += num;
        }
        
        // 如果不能整除，说明不能均分，返回 false
        if (sum % k != 0)
            return false;
        
        boolean[] visited = new boolean[nums.length];
        return dfs(nums, k, visited, 0, 0,sum/k);
    }

    private boolean dfs(int nums[], int k, boolean[] flags, int start, int curSum, int avg) {
        // 如果 k == 1 说明遍历到了最后一组，由于每组和相等，最后一组的和必定为 avg
        if(k == 1) return true;
        
        // 一个自己中元素和等于平均值时寻找下一子集
		if (avg == curSum) {
			return dfs(nums, k - 1, flags, 0, 0, avg);
		}

		for (int i = start; i < nums.length; i++) {
			if (!flags[i] && curSum + nums[i] <= avg) {
				flags[i] = true;
				if (dfs(nums, k, flags, i + 1, curSum + nums[i], avg)) {
					return true;
				}
				
				// 如果数组当前位置中的元素不能满足条件，重新将使用状态还原
				flags[i] = false;
			}
		}

		return false;
	}
}
```
