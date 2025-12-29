
## 算法原理

## 代码模版

## 典型题目
### 704-二分查找
1. 思路：题目中要求O(log n)的时间复杂度。所以就是二分。 一般O(log n)的时间复杂度的写法就是二分查找 或 二叉树。
2. 写代码：**不论哪种写法，理解并遵守代码执行的本质即可**。不论是[left,right]还是[left, right)，第二种写法就是right取不到，所以写代码的逻辑遵循right取不到即可。
3. 这个题目很base：因为nums都排好序，并且元素都不会重复。所以target只可能有一个值，或者没有。不会有多个元素等于target的情况。
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
### 35-搜索插入位置
1. 理解704题目的代码模版：之所以会退出循环，是因为nums数组中没有target。并且循环推出时left>right。
2. while循环退出前的2次循环体
   - 倒数第2次的while循环体：left==right-1，例如left=5, right=6。此时mid==left==5。不论走left+1还是right-1，都会导致left==right。
   - 倒数第1次的while循环体：left == right。例如left=5, right=5。此时mid==left==right。不论走left+1还是right-1，都会导致left > right【且left=right+1】。
     - 如果是left+1（left+1=6），则是因为nums[mid] < target，也就是nums[5]<target，target应该在的位置/索引是6。也就是left。
     - 如果是right-1（right-1=4），则是因为nums[mid] > target，也就是nums[5] > target，target应该在的位置/索引是5，也就是left。
```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int n = nums.size();
        if(n == 0) return 0;  // 如果是一个空数组，应该被插入到第1个位置
        int left = 0;
        int right = n - 1;
        int mid;
        while(left <= right){
            mid = (left + right) / 2;
            if(nums[mid] == target){
                return mid;
            }else if(nums[mid] > target){
                right = mid - 1;
            }else{
                left = mid + 1;
            }
        }
        // 不是left-1，不是right，
        // 而是left 或 right+1
        return left;  // return right+1;
        // 循环推出的条件是left > right。 出现这种情况是因为left==right，此时mid==left==right。所以不论right=mid-1还是left=mid+1，都会出现left > right。【此时target的位置应该是right或left-1】
        // 当right = left + 1，此时mid==left。 此时如果走left=mid+1，下一次循环就是left==right。所以一定是right=mid-1，下一次循环才是left > right。【target的位置应该是left-1】
        // 情况1: 最后一次循环是right = mid - 1;  此时nums[mid] > target。 所以target应该放到mid-1的位置，也就是right的位置。
        // 情况2：最后一次循环是left = mid + 1; 此时nums[mid] < target。 所以target应该在mid+1的位置。

    }
};
// 在二分的基础上，要求如果找不到，返回应该被插入的位置。而不是返回-1。
```


