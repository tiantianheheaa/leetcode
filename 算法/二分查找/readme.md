
## 算法原理

## 代码模版


## 题目总结
### 二分的模版
1. 704题是二分查找的模版：有序数组，每个元素只出现一次。[left, right]都是闭区间，因此while(left<=right) 因为left可以等于right，有这种情况。
2. 35题考察的是704题目循环退出时的边界。
3. 34题是704题目的多值情况。 也就是有序数组，每个元素可出现多次。找左边界和右边界也很简单，就是找打了就记录更新，然后继续寻找。
### 二分的应用
1. 69题是二分查找的应用。 将题目抽象为有序数组中值的查找，就可以用二分。
   - 367题-有效的完全平方数，和69题-x的平方根 一样，比69题还简单。
2. 50题-pow(x,y)
3. 162题 和 852题 都是峰值。
### 旋转排序数组（2段升序）
1. 33题是模版。
   - 81题是33题的多值情况，连续相等的时候，扰动就可以。
2. 153-旋转排序数组的最小值，是33题的理解。
  - 154题是153题的多值情况。

## 典型题目
### 704-二分查找（模板）
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
### 35-搜索插入位置（边界情况）
1. 思路：如果target在nums数组中，返回其位置/索引；如果target不在nums数组中，返回其应该被插入的位置。 **考察的是循环推出时，target应该被插入的位置**。
2. 理解704题目的代码模版：之所以会退出循环，是因为nums数组中没有target。**target的值夹在2个连续的nums索引中间**，例如nums[5]<target<nums[6]，才会退出循环。 并且**最后一次循环体一定是left==right==mid**。 基于这2点，进行以下分析：
   - 最后一次循环体是left==right==mid，如果left==right==mid==6，则target<nums[mid]，此时right=mid-1=5。target应该插入的位置是6，也就是left或者right+1。
   - 最后一次循环体是left==right==mid，如果left==right==mid==5，则target>nums[mid]，此时left=mid+1=6。target应该插入的位置是6，也就是left或right+1。
3. while循环退出前的2次循环体
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

### 34-排序数组中查找元素的第一个和最后一个位置（元素重复值）
1. 思路：整体思路非常简单，参考了官方题解下面的第一条评论，思路很简单。【有时候**学不会或者学着觉得难，是因为老师教的不好**，或者没有教一种最简单的理解方法。不要限制自己的思路，多看一些解法，打开思路，找到最简单最容易理解的一种解法。】
2. 在704模版的基础上，**nums[mid]==target时，不是return，而是记录/更新结果，然后继续寻找**。
<img width="458" height="412" alt="image" src="https://github.com/user-attachments/assets/ee5a7dcd-0fb7-4085-9c84-4631185ccacb" />

```cpp
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int n = nums.size();
        // if(n == 0) return [-1, -1];
        int left = 0;
        int right = n - 1;
        int mid;
        int left_res = -1;
        int right_res = -1;
        vector<int> res;

        while(left <= right){
            mid = (left+right) / 2;
            if(nums[mid] == target){
                // 不可以return，而是要把所有的while循环走完。
                left_res = mid; // 很重要，记录/更新一次左边界的结果
                right = mid - 1;  // 因为nums[mid]已经符合结果了，所以继续在[left, mid-1]区间找找看
            }else if(nums[mid] > target){
                right = mid - 1;
            }else{
                left = mid + 1;
            }
        }

        // 右侧
        left = 0;
        right = n-1;
        while(left <= right){
            mid = (left+right) / 2;
            if(nums[mid] == target){
                // 不可以return，而是要把所有while循环走完。
                right_res = mid;  // 记录/更新一次右边界的结果
                left = mid + 1;  // nums[mid]已经符合结果了，继续在[mid+1, right]区间找找看
            }else if(nums[mid] > target){
                right = mid - 1;
            }else{
                left = mid + 1;
            }
        }

        res.push_back(left_res);
        res.push_back(right_res);
        return res;
    }
};
// 可能有多个target的位置。
// 最左边或最右边，就需要left=mid和right=mid了。 
```

### 69-x的平方根（二分的应用）
1. 思路：**任何一个有序数组中，值的查找，都可以用二分**。 本题就是在1到x的有序数组中遍历，查找target的值。从而可以用二分查找。
2. 平方根的定义: 例如res是x的平方根，则res*res <= x && (res+1) * (res+1)> x。
```cpp
class Solution {
public:
    int mySqrt(int x) {
        // 边界情况
        if(x == 0 || x == 1) return x; 
        // 常规逻辑
        int left = 1;
        int right = x/2+1;  // 只有1*1=1，2*2=4，从3开始，平方根都小于x/2
        int mid;
        while(left <= right){
            mid = (left+right) / 2;  // 或者这里改成left+(right-left)/2，不会溢出。
            // 第一种情况就是平方根的定义。
            if((long long)mid*mid <= x && (long long)(mid+1)*(mid+1) > x){
                return mid;
            }else if((long long)mid*mid < x){
                left = mid + 1;
            }else{
                right = mid - 1;
            }
        }
        return -1;
    }
};
// 平方根的定义：res的平方，小于等于x。 res+1的平方，大于x。
// 遍历查找：从1到x，遍历查找。 可以得到target的位置。    
// 二分查找:也可以。
// 【任何一个有序数组的查找，都可以用二分】 上面的1到x的数组，就是一个有序数组。
```

### 50-pow(x,y) （二分的应用）
1. 思路：直接乘法是n次x的相乘。通过二分，转化为log n次的乘法计算。
2. 举个例子：枚举的过程中解题思路就出来了。
3. 快速幂：就是递归+二分，时间复杂度和空间复杂度都是O(log n)，这也是递归函数调用的次数。 空间复杂度=栈的深度=递归调用次数。
4. 进阶：位运算，空间复杂度可以降低为O(1)。
<img width="442" height="430" alt="image" src="https://github.com/user-attachments/assets/b01a39a7-28e9-404b-a23d-c4244517dde8" />

```cpp
class Solution {
public:
    double myPow(double x, int n) {
        if(n >= 0){
            return dfs(x, n);
        }else{
            return 1/dfs(x, n);
        }
    }
    double dfs(double x, int n){
        // 任何数的0次幂，都是1
        if(n == 0){
            return 1;
        }
        double tmp_res = dfs(x, n/2);  // n/2就是二分，每次都砍半。所以一共是log n次递归。
        if(n % 2 == 0){
            return tmp_res * tmp_res;
        }else{
            return tmp_res * tmp_res * x;
        }
    }
};
// 可以用二分来节省，不用算n次乘法。  每次都除以2，所以log n次乘法就可以了。
// 可以用递归，递归边界是n=1，也就是x本身。

// 一个有序数组的查找
// 快速幂，就是专门用于这个题目的pow
```

### 33-搜索旋转排序数组（模版）
1. 思路：升序数组被选择后，**分为2段有序数组**。 而且第2段的最大值 < 第1段的最小值。
   - 如果mid落在了第1段（即mid1的位置）
      - 如果target的值在left和mid1之间，则right = mid - 1; 继续查找[left, mid1]。
      - 如果target不在left到mid1之间，则target的值在后面2个分段函数，此时left = mid + 1; 继续找后面的2个分段函数。
   - 如果mid落在了第2段（即mid2的位置）
      - 如果target在mid2和right之间，则left = mid + 1；继续查找[mid2, right]。
      - 如果target不在mid2和right之间，则在前面的2个分段函数，此时right = mid - 1; 继续查找前面2个分段函数。
<img width="412" height="370" alt="image" src="https://github.com/user-attachments/assets/21fd8512-3284-4c3b-b159-b4cbb613c948" />

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int n = nums.size();
        int left = 0;
        int right = n-1;
        int mid;
        while(left <= right){
            mid = (left + right) / 2;
            if(nums[mid] == target){
                return mid;
            }
            // 升序数组由于旋转，导致分为了2段，每一段都是升序。并且第2段的最大值 < 第1段的最小值
            // 先分段，看mid的位置落在了左半段，还是右半段
            if(nums[mid] >= nums[left]){
                if(target >= nums[left] && target <= nums[mid]){
                    right = mid - 1;
                }else{
                    left = mid + 1;
                }
            }else{
                if(target >= nums[mid] && target <= nums[right]){
                    left = mid + 1;
                }else{
                    right = mid - 1;
                }
            }
        }
        return -1;
    }
};
// 一个有序数组，进行了旋转。 用二分来查找
```
### 81-搜索旋转排序数组2（有重复值）
1. 思路：和33题目的思路是一样的。就是多了一种情况讨论：连续多个相同的值，此时区间判断可能会出错。所以把nums[mid]==nums[left]单独拿出来讨论，通过left++扰动出重复值区间。
```cpp
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        int n = nums.size();
        int left = 0;
        int right = n - 1;
        int mid;
        while(left <= right){
            mid = (left + right) / 2;
            if(nums[mid] == target){
                return true;
            }
            // 因为下面的if...else..的条件必有一个有等于的边界，当nums[left]==nums[mid]时， 区间判断会出错。
            if(nums[mid] == nums[left]){
                left ++;  // 扰动区间
                continue; 
            }
            // 下面的if...else...的模版是没变的。 就是当nums[mid]==nums[left]时，可能会判断错区间。所以把==这种情况，在上面单独拎出来讨论。
            if(nums[mid] >= nums[left]){
                if(target >= nums[left] && target <= nums[mid]){
                    right = mid - 1;
                }else{
                    left = mid + 1;
                }
            }else{
                if(target >= nums[mid] && target <= nums[right]){
                    left = mid + 1;
                }else{
                    right = mid - 1;
                }
            }
        }
        return false;
    }
};
```
### 153-旋转排序数组的最小值（对33题的理解）
1. 思路：对33题的理解。还是33题的图。
   - 最小值：第2段区间的起点。
   - 最大值：第1段区间的末尾。
2. **不论是求最大值还是最小值，就往目标值附近逼近**。  不是求target值，不需要if...else...4种情况。
3. 求最小值
   - 当mid在第2段区间，right=mid。（不用right=mid-1，因为mid可能是最小值）
   - 当mid在第1段区间，left = mid +1。
4. 求最大值
   - 当mid在第1段区间，left = mid。（不用left=mid+1，因为mid可能是最大值）
   - 当mid在第2段区间，right = mid - 1。
<img width="412" height="370" alt="image" src="https://github.com/user-attachments/assets/21fd8512-3284-4c3b-b159-b4cbb613c948" />

```cpp
// 最小值
class Solution {
public:
    int findMin(vector<int>& nums) {
        int n = nums.size();
        int left = 0;
        int right = n - 1;
        int mid;
        while(left < right){
            mid = (left + right) / 2;
            // 如果mid在第2段区间，则right=mid。 因为mid可能是最小值，所以不是right=mid-1。
            if(nums[mid] < nums[right]){  // 这里nums[mid] <= nums[right]也可以
                right = mid;
            }else{ // mid在第1段区间，则left=mid+1。
                left = mid + 1;
            }
        }
        return nums[right];
    }
};
// 最小值是第2段区间的起点。 逐渐向中间逼近即可。【思路是没问题的】
// 两种写法。 
// 求最大值：第1段区间的终点
// 求最小值：第2段区间的起点。
```
### 154-旋转排序数组的最小值2（有重复值）
1. 思路：有重复值，会导致153题目的模版中 nums[mid]==nums[right]的情况下，区间判断出错。 所以当nums[mid] == nums[right]时，通过right--来绕动出来即可。
```cpp
class Solution {
public:
    int findMin(vector<int>& nums) {
        int n = nums.size();
        int left = 0;
        int right = n - 1;
        int mid;
        while(left < right){
            mid = (left + right) / 2;
            // 相等时，扰动。 保证下面的区间判断是正确的
            if(nums[mid] == nums[right]){
                right--;  // 还是用扰动的方法，就可以
                continue;
            }
            // nums[mid] == nums[right]时，在重复值的情况下，会判断出错。所以单独讨论来处理。right--
            if(nums[mid] <= nums[right]){
                right = mid;
            }else{
                left = mid + 1;
            }
        }
        return nums[right];
    }
};
```
### 162-寻找峰值（二分的应用）
1. 二分的应用：二分的思想很简单：（1）while循环的边界初始化值 left 和right分别位于待搜寻的数组边界。 （2）循环体中是if...else...的2种或3种互斥的情况，不同情况的处理有left++和right--，从而left和right才能逼近循环退出边界。
2. 峰值的情况，就符合二分使用场景的定义：（1）峰值的定义，是**严格大于左右相邻的元素**。（2）画图可以发现，不是峰值的情况，就2种。mid在上山 或 mid在下山，分别对应left++和right--的情况。
3. 易错点：题目中说的严格大于左右相邻元素，是不正确的。因为测试用例中给了[1,2,3,4]这样的单边峰，结果是nums[3]=4。 所以**单边峰**也是符合题目定义的。
<img width="406" height="388" alt="image" src="https://github.com/user-attachments/assets/f347c62e-6429-4c90-b04a-331d55da61e6" />

```cpp
// 通过62/72。思路是对的。
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int n = nums.size();
        // 边界情况的处理
        if(n == 1){
            return 0;
        }
        if(n == 2){
            if(nums[0] > nums[1]){
                return 0;
            }else{
                return 1;
            }
        }
        // 常规逻辑
        int left = 0;
        int right = n - 1;
        int mid;
        
        while(left < right){
            mid = (left + right) / 2;
            // mid-1和mid+1有效性的判断
            if(mid-1 < left || mid+1 > right){
                return mid;
            }
            // 3个情况讨论
            // 可以通过62/72。无法通过的例子是单边峰，例如[1,2,3,4]
            if(nums[mid] > nums[mid-1] && nums[mid] > nums[mid+1]){
                return mid;
            }else if(nums[mid] <= nums[mid-1]){
                right = mid - 1;  // 如果是单边峰，right=mid也可以。
            }else{
                left = mid + 1;  // 如果是单边峰，left=mid也可以。
            }
        }
        return right;
    }
};
// 多个峰值。 返回任意一个即可。
// 二分的过程中，如果

// 峰值的定义是 左右相邻元素。
// 思路是对的： 二分的使用，就是这样。 if...else..分为2种或3种情况，然后需要有left++和right--，最终才能逐渐left逼近right，退出while循环。
```

```cpp
// 单边峰的写法。可以通过全部示例。
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int n = nums.size();
        int left = 0;
        int right = n - 1;
        int mid;
        while(left < right){
            mid = (left + right) / 2;
            // mid只和单边mid+1比较。 因为有的case是单边峰，例如[1,2,3,4]
            if(nums[mid] > nums[mid+1]){
                // nums[mid] > nums[mid+1]，此时mid可能为峰值。
                right = mid;
            }else{
                // nums[mid] <= nums[mid+1]，此时mid一定不是峰值。因为峰值是严格大于。
                left = mid + 1;
            }
        }
        // 最终return left 或 right都可以。因为循环推出的条件是left == right
        return right;
    }
};
// 单边峰的思路
```

### 852-山脉数组的峰值索引（二分的应用，同162题）
1. 比162题简单，这个题目只有1个峰值。162题可能有多个峰值。
2. 思路：和162题目一样。利用**峰值的一半定义**即可（峰顶的元素>其右边的元素）。 从而if...else...分2种情况讨论即可，不用分3种情况讨论。
3. 对于while(left < right)，循环退出的条件一定是left==right，此时return left或right都可以。
```cpp
class Solution {
public:
    int peakIndexInMountainArray(vector<int>& arr) {
        int n = arr.size();
        int left = 0;
        int right = n - 1;
        int mid;
        while(left < right){
            mid = (left + right) / 2;
            // 靠着单峰的定义也可以。
            if(arr[mid] > arr[mid+1]){
                // arr[mid] > arr[mid+1]，此时mid可能是峰顶，所以不能right=mid-1。
                right = mid;
            }else{
                // arr[mid] <= arr[mid+1]，mid一定不是峰顶。
                left = mid + 1;
            }
        }
        // return left和right都可以，循环退出的时候left==right。
        return right;
    }
};
// 这是严格
```
