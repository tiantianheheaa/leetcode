
# 算法原理、代码模版、典型题目
## 冒泡排序
## 归并排序和快排
1. 归并排序和快排的思路和代码都非常一致，建议一起记忆。思路都是分治（递归求解）+子问题的处理（双指针求解）。
2. 归并排序的思路
- 递归：int mid = (left+right)/2，每次都是准确的二分。然后dfs(vector,left,mid) 和dfs(vector, mid+1, right)。
- 子问题的处理：合并2个有序数组。
  - 思路也非常简单自然，双指针left和right分别指向2个子数组的左边界，然后同向向右移动。如果vector[left] <= vector[right]，就把vector[left]添加到tmp数组中，反之把vector[right]添加到tmp数组。 最后利用tmp数组修改vector数组的值，使得vector有序。
```cpp
class Solution {
public:
    vector<int> sortArray(vector<int>& nums) {
        int size = nums.size();
        dfs(nums, 0, size-1);
        return nums;
    }
    void dfs(vector<int>& nums, int left ,int right){
        // 数组只有1个元素，是没法分为2个数组的。
        if(left == right){
            return;
        }
        int mid = (left + right) / 2;
        dfs(nums, left, mid);
        dfs(nums, mid+1, right);
        merge(nums, left, mid, mid+1, right);
    }
    void merge(vector<int>& nums, int left_left, int left_right, int right_left, int right_right){
        vector<int> tmp;
        int left = left_left;
        int right = right_left;
        while(left <= left_right && right <= right_right){
            if(nums[left] <= nums[right]){
                tmp.push_back(nums[left]);
                left++;
            }else{
                tmp.push_back(nums[right]);
                right++;
            }
        }
        while(left <= left_right){
            tmp.push_back(nums[left]);
            left++;
        }
        while(right <= right_right){
            tmp.push_back(nums[right]);
            right++;
        }
        for(int i = 0; i < tmp.size(); i++){
            nums[left_left+i] = tmp[i];
        }
    }
};
// 归并排序的思路：（1）递归分治两个子数组。（2）合并两个有序子数组。
// 数组引用 + 下标，是很好的一种方式。
    // 是减少数据传递的最好的方式了。
    // 合并2个有序子数组，tmp数组的空间复杂度是不可避免的。
// 第二部分：合并2个子数组是很容易的思路，很容易写出来。
```
4. 快排的思路
- 递归
  - int p = partition(vector, left, right)，下标p将vector数组分为2部分，使得p左边的数都小于等于vector[p]，p右边的数都大于vector[p]。
  - 然后dfs(vector, left, p) 和 dfs(vector, p+1, right)。
- 子问题的处理：找到下标p，使得其左边的数都小于等于vector[p]，其右边的数都大于等于vector[p]。
  - 逆向双指针，分别从vector的左端和右端出发。tmp是vector[left]的值。如果vector[right] > tmp，right--，否则vector[right]覆盖vector[left]的值。 如果vector[left]<=tmp，left++，否则vector[left]覆盖vector[right]的值。
```cpp
class Solution {
public:
    vector<int> sortArray(vector<int>& nums) {
        int size = nums.size();
        dfs(nums, 0, size-1);
        return nums;
    }
    void dfs(vector<int>& nums, int left, int right){
        // 因为有p+1，所以可能left > right。
        if(left >= right){
            return;
        }
        int p = partition(nums, left, right);
        dfs(nums, left, p-1);
        dfs(nums, p+1, right);
    }
    int partition(vector<int>& nums, int left, int right){
        // 把元素nums[left]放到一个合适的位置，使得其左边的元素都小于等于它，右边的元素都大于它。
        // 不加随机主元，会超时。
        int rand_idx = left + rand() % (right-left+1);  // rand_idx的范围是[left, right]。
        swap(nums[left], nums[rand_idx]);
        int tmp = nums[left];
        while(left < right){
            while(left < right && nums[right] > tmp) right--;
            nums[left] = nums[right];
            while(left < right && nums[left] <= tmp) left++;
            nums[right] = nums[left];
        }
        nums[left] = tmp;
        return left;
    }
};
// 快排 整体的思路 和 归并排序是一样的。 dfs + 子函数。
// dfs的不同：归并排序的边界条件是left==right。 快排由于p+1的操作，所以边界条件是left >= right。
// 子函数中都用到了双指针。
// 归并排序的子函数是合并2个有序数组，比较简单。同向双指针。
// 快排的子函数是寻找partition分割2个子数组。找个算法笔记书中的例子的过程，就可以写出来，先right-- 因为要覆盖left的位置。left本身的值已经存储到tmp变量中了。
// 快排的子函数不加随机主元，会超时。
```
5. 对比归并排序和快排。
- 递归部分
  - 递归边界：归并排序:left == right。快排是left >= right。
  - 递归：都是dfs左右2个子数组。 归并排序由于准确二分，可以保证O(n*logn)的时间复杂度。快排通过p的位置来二分，左右不一定严格对称，所以子函数中通过**随机主元**的做法，使得平均时间复杂度是O(n * logn)。
- 子问题的求解
  - 归并排序使用同向双指针，思路自然简单。
  - 快排使用逆向双指针，相对归并排序稍微麻烦一点，思路需要记忆，不容易想到。 且需要随机主元来初始化。  
6. 归并排序的扩展题目：逆序对。
- 思路：通过递归求解数组的逆序对 = 2个子数组的逆序对 + 2个子数组间的逆序对。
  - dfs和子问题的求解，依然是对数组进行排序，因为有序数组才容易计算2个子数组间的逆序对，例如左边数组某个元素a大于右边数组b，则a元素右边的元素都大于右边数组的b。   
- 代码：在归并排序的基础上，增加一行代码即可。（在子问题求解的函数中增加int count=mid-left+1来计算2个子数组间的逆序对）
```cpp
class Solution {
public:
    int reversePairs(vector<int>& record) {
        int size = record.size();
        return dfs(record, 0, size-1);
    }
    int dfs(vector<int>& record, int left, int right){
        if(left >= right){
            return 0;
        }
        int mid = (left + right) / 2;
        // count就是逆序对的数据，是三部分组成的。count=2个子数组内各自的逆序对数量+ 两个子数组间构成的逆序对数量。
        int count = dfs(record, left, mid) + dfs(record, mid+1, right);
        count += merge(record, left, mid, mid+1, right);
        return count;
    }
    int merge(vector<int>& record, int left_left, int left_right, int right_left, int right_right){
        vector<int> tmp;
        int left = left_left;
        int right = right_left;
        int count = 0;
        while(left <= left_right && right <= right_right){
            if(record[left] <= record[right]){
                tmp.push_back(record[left]);
                left++;
            }else{
                tmp.push_back(record[right]);
                right++;
                // 多这一行代码。 计算2个子数组间的逆序对的数量。
                count += left_right - left + 1;
            }
        }
        while(left <= left_right){
            tmp.push_back(record[left]);
            left ++;
        }
        while(right <= right_right){
            tmp.push_back(record[right]);
            right ++;
        }
        for(int i = 0; i < tmp.size(); i++){
            record[left_left+i] = tmp[i];
        }
        return count;
    }
};
// 在归并排序的基础上，增加了一行代码，计算2个子数组间的逆序对的数量。
// nice一次就通过了。
```
