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




