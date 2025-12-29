
## 算法原理

## 代码模版

## 典型题目
### 704-二分查找
1. 思路：题目中要求O(log n)的时间复杂度。所以就是二分。 一般O(log n)的时间复杂度的写法就是二分查找 或 二叉树。
2. 写代码：**不论哪种写法，理解并遵守代码执行的本质即可**。不论是[left,right]还是[left, right)，第二种写法就是right取不到，所以写代码的逻辑遵循right取不到即可。
```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        // 边界情况
        int n = nums.size();
        if(n == 0) return -1;
        // 常规逻辑
        int left = 0;
        int right = n-1;
        int mid;
        // 因为left和right两个边界都可以取到，并且left==right是可能存在这种情况的。 并且while循环退出后，直接return -1; 所以while循环中要把所有正确的情况都考虑了。
        // left==right，此时mid==left==right，就是要判断这个值nums[mid]是否等于target。所以这种情况在while循环中一定要判断到。
        while(left <= right){
            // 每一次循环，mid都需要更新
            mid = (left+right) / 2;
            // left和right的更新是在if...else...中
            if(nums[mid] == target){
                return mid;
            }else if(nums[mid] > target){
                // 上面的mid已经判断过，所以下面判断mid-1或mid+1即可。
                right = mid-1;
            }else{
                left  = mid + 1;
            }
        }
        return -1;
    }
};
// 二分查找的前提是一个有序的数组，时间复杂度是O(log n)。
// 二分是双指针的一种， 只不是每次不是走一步，而是走一半。

// O(log n)的时间复杂度，就是二分查找 或者 二叉树。因为每次都节省一半。
// 返回val=target对应的下标，还有的题目 需要返回最左边的下标 或 最右边的下标。
```
