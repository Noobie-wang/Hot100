# Hot100
Two questions per day

# 1.两数之和

给定一个整数数组 `nums` 和一个整数目标值 `target`，请你在该数组中找出 **和为目标值** *`target`* 的那 **两个** 整数，并返回它们的数组下标。你可以假设每种输入只会对应一个答案，并且你不能使用两次相同的元素。你可以按任意顺序返回答案。

 **示例 1：**

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

**示例 2：**

```
输入：nums = [3,2,4], target = 6
输出：[1,2]
```

**示例 3：**

```
输入：nums = [3,3], target = 6
输出：[0,1]
```

 **提示：**

- `2 <= nums.length <= 104`
- `-109 <= nums[i] <= 109`
- `-109 <= target <= 109`
- **只会存在一个有效答案**

 

**进阶：**你可以想出一个时间复杂度小于 `O(n2)` 的算法吗？

### 一、暴力求解

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        for(int i = 0; i<=nums.size()-1;i++){
            for (int j = i+1;j<=nums.size()-1;j++){
                if (nums[i]+nums[j]==target){
                    return {i,j};
                }
            }
        }
        return {};
    }
};
```

此时的算法复杂度为`O(n^2)` 如何进行优化呢？

使用哈希表进行优化

### 二、哈希求解

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
     	unordered_map<int,int> hashtable; #创建哈希表
        for(int i = 0; i < nums.size(); i++){
            int need = target - nums[i];
            auto it = hashtable.find(need); #auto 自动推导类型 it 迭代器 
            if(it != hashtable.end()){ #如果need不在末尾 说明找到了
                return {it->second,i}; #返回下标
            }
            hashtable[nums[i]]=i;
        }
        return {};
    }
};
```

### 周日复习版

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
     	unordered_map<int,int> hash;
        for(int i = 0; i < nums.size(); i++){
            int need = target - nums[i];
            auto it = hash.find(need); //忘记了使用迭代器 而是直接使用下面的部分
            if(hash.find(need) != hash.end() ){
                return {hash(need),i}; //这里错误的使用了hash的读取方式 正确的应该是hash[need]
            }
            hash[nums[i]]=i;
        }
        return {};
    }
};
```



## 49.字母异位词分组

给你一个字符串数组，请你将 字母异位词 组合在一起。可以按任意顺序返回结果列表。

 **示例 1:**

**输入:** strs = ["eat", "tea", "tan", "ate", "nat", "bat"]

**输出:** [["bat"],["nat","tan"],["ate","eat","tea"]]

**解释：**

- 在 strs 中没有字符串可以通过重新排列来形成 `"bat"`。
- 字符串 `"nat"` 和 `"tan"` 是字母异位词，因为它们可以重新排列以形成彼此。
- 字符串 `"ate"` ，`"eat"` 和 `"tea"` 是字母异位词，因为它们可以重新排列以形成彼此。

**示例 2:**

**输入:** strs = [""]

**输出:** [[""]]

**示例 3:**

**输入:** strs = ["a"]

**输出:** [["a"]]

 **提示：**

- `1 <= strs.length <= 104`
- `0 <= strs[i].length <= 100`
- `strs[i]` 仅包含小写字母

```c++
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string,vector<string>> hash;   //创建哈希表
        for(string s : strs){
            string key = s; //将遍历的字符串进行复制 进行后续操作
            sort(key.begin(),key.end()); //对复制的字符串进行排序
            hash[key].push_back(s); //将字符串放在哈希表中
        }
        vector<vector<string>> result; //创建双层数组result
        for (auto& pair : hash){  //auto pair 会复制原数据 auto& pair引用数据 效率更高 
            result.push_back(pair.second); //pair中有key与value second输出对应的value值
        }
        return result;
    }
};
```

### 周日复习版

```c++
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string,vector<string>> hash;

        for(string s : strs){
            string key = s;
            sort(key.begin(),key.end()); //忘记了排序 忘记了如何排序
            hash[key].push_back(s); //push_back的前后都不知道 不知道push_back谁 也不是到给谁push_back
        }
        vector<vector<string>> result;
        for(auto&pair : hash){ //忘记了如何遍历 遍历谁 如何遍历hash
            result.push_back(pair.second);
        }
        return result;
    }
};
```



## 128.最长连续序列

给定一个未排序的整数数组 `nums` ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。请你设计并实现时间复杂度为 `O(n)` 的算法解决此问题。

 **示例 1：**

```
输入：nums = [100,4,200,1,3,2]
输出：4
解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
```

**示例 2：**

```
输入：nums = [0,3,7,2,5,8,4,6,0,1]
输出：9
```

**示例 3：**

```
输入：nums = [1,0,1,2]
输出：3
```

 **提示：**

- `0 <= nums.length <= 105`
- `-109 <= nums[i] <= 109`



第一反应是排序 然后遍历 直接出结果 但是题目限定了算法复杂度需要为`O(n)` 而排序算法的算法复杂度为`O(nlogn)` 所以要采取其他方法 可以采用哈希法

```c++
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int> set;
        for(int num : nums){
            set.insert(num); //insert可以直接去重
        }
        int result = 0;
        for(int num : set){
            if(set.find(num-1)==set.end()){ //证明是起点 未发现前面还有 所以=end
                int current = num;
                int length = 1;
                while(set.find(current+1)!=set.end()){
                    current++;
                    length++;
                }
                result = max(result,length);
            }
        }
        return result;
    }
};
```

### 周日复习版

```c++
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int> set;
        for(int num:nums){
            set.insert(num); //这一块直接忘掉 忘记给set赋值
        }
        int result = 0;
        for(int num : set){
            if(set.find(num-1)==set.end()){ //通过find以及和end()比较的形式来确定是否查找到
                int current = num;
                int length = 1; //这里长度直接为1了
                while(set.find(current+1)!=set.end()){//这里进行判断和比较的是current 而非num了
                    current++;
                    length++;
                }
                result = max(result,length);
            }
        }
        return result;
    }
};
```


## 283.移动零

给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。

**请注意** ，必须在不复制数组的情况下原地对数组进行操作。

**示例 1:**

```
输入: nums = [0,1,0,3,12]
输出: [1,3,12,0,0]
```

**示例 2:**

```
输入: nums = [0]
输出: [0]
```

**提示**:

- `1 <= nums.length <= 104`
- `-231 <= nums[i] <= 231 - 1`

**进阶：**你能尽量减少完成的操作次数吗？

#### 使用双指针

```c++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int slow = 0;
        for (int fast = 0; fast<nums.size(); fast++){
            if (nums[fast]!=0){
                swap(nums[slow],nums[fast]);
                slow++;
            }
        }
    }
};
```

### 周日复习版

### 通过

## 11.盛最多水的容器

给定一个长度为 `n` 的整数数组 `height` 。有 `n` 条垂线，第 `i` 条线的两个端点是 `(i, 0)` 和 `(i, height[i])` 。找出其中的两条线，使得它们与 `x` 轴共同构成的容器可以容纳最多的水。返回容器可以储存的最大水量。

**说明：**你不能倾斜容器。

**示例 1：**

![img](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/07/25/question_11.jpg)

```
输入：[1,8,6,2,5,4,8,3,7]
输出：49 
解释：图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。
```

**示例 2：**

```
输入：height = [1,1]
输出：1
```

**提示：**

- `n == height.length`
- `2 <= n <= 105`
- `0 <= height[i] <= 104`

 

```c++
class Solution {
public:
    int maxArea(vector<int>& height) {
        int left = 0;
        int right = height.size()-1;
        int result = 0;
        while(left < right){
            int area = (right - left) * min(height[right],height[left]);
            result = max(result,area);
            if(height[left] < height[right]){
                left++;
            }
            else{
                right--;
            }
        }
        return result;
    }
};
```

## 15.三数之和

给你一个整数数组 `nums` ，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k` ，同时还满足 `nums[i] + nums[j] + nums[k] == 0` 。请你返回所有和为 `0` 且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。

**示例 1：**

```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
解释：
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
注意，输出的顺序和三元组的顺序并不重要。
```

**示例 2：**

```
输入：nums = [0,1,1]
输出：[]
解释：唯一可能的三元组和不为 0 。
```

**示例 3：**

```
输入：nums = [0,0,0]
输出：[[0,0,0]]
解释：唯一可能的三元组和为 0 。
```

**提示：**

- `3 <= nums.length <= 3000`
- `-105 <= nums[i] <= 105`

### 排序＋双指针 去重是关键

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        sort(nums.begin(),nums.end());
        for(int i = 0; i < nums.size()-2; i++){
            //先给第一个去重
            if(i > 0 && nums[i]==nums[i-1]){
                    continue;
                }
            if(nums[i]>0){
                    break;
                }
            int left = i+1;
            int right = nums.size()-1;
            while(left<right){
                int n = nums[i]+nums[left]+nums[right];
                if(n==0){
                    result.push_back({nums[i],nums[left],nums[right]});
                    while(left < right && nums[left]==nums[left+1]){
                        left++;
                    }
                    while(left < right && nums[right]==nums[right-1]){
                        right--;
                    }
                    left++;
                    right--;
                }
                if(n<0){
                    left++;
                }
                if(n>0){
                    right--;
                }
            }
        }
        return result;
    }
};
```

## 3.无重复字符的最长子串

给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长 子串** 的长度。

**示例 1:**

```
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。注意 "bca" 和 "cab" 也是正确答案。
```

**示例 2:**

```
输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

**示例 3:**

```
输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

**提示：**

- `0 <= s.length <= 105`
- `s` 由英文字母、数字、符号和空格组成

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_set<char> set;
        int left = 0;
        int result = 0;
        for(int right = 0; right<s.size();right++){
            while(set.find(s[right])!=set.end()){
                set.erase(s[left]);
                left++;
            }
            set.insert(s[right]);
            result = max(result,right-left+1);
        }
        return result;
    }
};
```


## 206.反转链表

给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)

```
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)

```
输入：head = [1,2]
输出：[2,1]
```

**示例 3：**

```
输入：head = []
输出：[]
```

**提示：**

- 链表中节点的数目范围是 `[0, 5000]`
- `-5000 <= Node.val <= 5000`

 

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* prev = nullptr;
        ListNode* curr = head;
        while(curr){
            ListNode*next = curr->next;
            curr->next = prev;
            prev = curr;
            curr = next;
        }
        return prev;
    }
};
```
## 438.找到字符串中所有字母异位词

给定两个字符串 `s` 和 `p`，找到 `s` 中所有 `p` 的 **异位词** 的子串，返回这些子串的起始索引。不考虑答案输出的顺序。

**示例 1:**

```
输入: s = "cbaebabacd", p = "abc"
输出: [0,6]
解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的异位词。
```

 **示例 2:**

```
输入: s = "abab", p = "ab"
输出: [0,1,2]
解释:
起始索引等于 0 的子串是 "ab", 它是 "ab" 的异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的异位词。
```

**提示:**

- `1 <= s.length, p.length <= 3 * 104`
- `s` 和 `p` 仅包含小写字母

```c++
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        vector<int> result;
        if(s.size()<p.size()){
            return result;
        }
        vector<int> pcount(26,0);
        vector<int> windowcount(26,0);
        for(char c : p){
            pcount[c - 'a']++;
        }
        int left = 0;
        for(int right = 0; right < s.size(); right++){
            windowcount[s[right] - 'a']++;
            if(right - left + 1 > p.size()){
                windowcount[s[left] - 'a']--;
                left++;
            }
            if(right - left + 1 == p.size()){
                if(pcount == windowcount){
                    result.push_back(left);
                }
            }
        }
        return result;
    }
};
```

## 560.和为 K 的子数组

给你一个整数数组 `nums` 和一个整数 `k` ，请你统计并返回 *该数组中和为 `k` 的子数组的个数* 。子数组是数组中元素的连续非空序列。

**示例 1：**

```
输入：nums = [1,1,1], k = 2
输出：2
```

**示例 2：**

```
输入：nums = [1,2,3], k = 3
输出：2
```

**提示：**

- `1 <= nums.length <= 2 * 104`
- `-1000 <= nums[i] <= 1000`
- `-107 <= k <= 107`

### 首先想到的是暴力

```c++
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        int result = 0;
        for(int left = 0;left<nums.size();left++){
            int sum = 0;
            for(int right = left; right < nums.size(); right++){
                sum += nums[right];
                if(sum == k){
                    result++;
                }
            }
        }
        return result;
    }
};
```

但是复杂度太高了 超出时间限制

### 然后使用前缀和 通过转换 然后转化为了两数之和  后面的前缀和--前面的前缀和=目标k 因此是前缀和+哈希

```c++
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        unordered_map<int,int> hash;
        hash[0] = 1;
        int sum = 0;
        int result = 0;
        for(int num : nums){
            sum += num;
            if(hash.find(sum-k)!=hash.end()){
                result += hash[sum - k];
            }
            hash[sum]++;
        }
        return result;
    }
};
```






