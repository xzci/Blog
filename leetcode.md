# leetcode 再刷。

其实应该算是第一次了。暴露了很多的问题。

## <-E> 1.Two Sum 286ms 2.67%

暴力方法，但是过了。
原题就是给出一个数值，然后再数组中，找到2个元素，相加的值恰好为给定的值，那么返回这2个元素的下标的集合。
普通思路其实也是可以做的。（2次循环，逐个加和判断是否相等。）但是应该还是有相应的简单的解法。
我的解法：（普通思路）

```C++
vector<int> twoSum(vector<int>& nums, int target) {
    vector <int> answer;
    for (auto i = 0; i != nums.size(); i++)
    {
        for (auto j = i + 1; j != nums.size(); j++)
        {
            if (nums[i] + nums[j] == target)
            {
                answer.push_back(i);
                answer.push_back(j);
            }
        }
    }
    return answer;
}
```

大佬的解法：
利用hashmap的无序性。 `hash.find(numberToFind) != hash.end()` 如果没有找到，则将元素的值作为hash的key， 元素的位数为值添加到map中。以此类推直到找到该元素为止。

```C++
vector<int> twoSum(vector<int> &numbers, int target)
{
    //Key is the number and value is its index in the vector.
    unordered_map<int, int> hash;
    vector<int> result;
    for (int i = 0; i < numbers.size(); i++) {
        int numberToFind = target - numbers[i];
        // if numberToFind is found in map, return them
        if (hash.find(numberToFind) != hash.end()) {
            result.push_back(hash[numberToFind]);
            result.push_back(i);
            return result;
        }
        // number was not found. Put it in the map.
        hash[numbers[i]] = i;
    }
    return result;
}
```

### 展开

使用`unordered_map`来做。 该数据结构被定义在`unordered_map` 库中。

* unordered_map 内部实现为哈希表。（所以是无序的）
* map 内部实现为红黑树（所以map内部的元素都是有序的）

#### map

优点：

* 有序性
* 内部由红黑树实现（使用红黑树特性的时候，效率快）

缺点：

* 空间占用率高（节点）

#### unordered_map

优点：

* 查找快

缺点：

* 建立hash表的时候，消耗大量时间

## <-M> 2.Add Two Numbers 56ms 27.5%

使用链表计算加法。
熟练使用链表就行。（思路相同，根据大佬的代码风格整理了一下链表实现的方式）

```C++
(l1 ? l1->val : 0)
l1 = l1 ? l1->next : l1;
```

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        int remainder = 0;
        ListNode answer(0);
        ListNode *p = &answer;
        while (l1 || l2 || remainder) {
            int sum = (l1 ? l1->val : 0) + (l2 ? l2->val : 0) + remainder;
            remainder = sum / 10;
            p->next = new ListNode(sum % 10);
            p = p->next;
            l1 = l1 ? l1->next : l1;
            l2 = l2 ? l2->next : l2;
        }
        return answer.next;
    }
};
```

## <-M> 3.Longest Substring Without Repeating Characters 222ms 12.95%

```C++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int ans = 0;
        vector<-char> check;
        for (auto i = s.begin(); i != s.end(); i++)
        {
            for (auto j = i; j != s.end(); j++)
            {
                if (find(check.begin(), check.end(), *j) == check.end())
                    check.push_back(*j);
                else
                    break;
            }
            if (ans <- check.size())
            {
                ans = check.size();
                check.clear();
            }
            else
            {
                check.clear();
            }
        }
        return ans;
    }
};
```

## <-H> 4.Median of Two Sorted Arrays 62ms 38%

This solution is not very good, just use sort to get the answer, but the answer still great then other 62% people's.:P

```C++
class Solution {
public:
    double findMedianSortedArrays(vector<-int>& nums1, vector<-int>& nums2) {
        vector<-int> Temp;
        for (int i = 0; i != nums1.size(); i++)
        {
            Temp.push_back(nums1[i]);
        }
        for (int i = 0; i != nums2.size(); i++)
        {
            Temp.push_back(nums2[i]);
        }
        sort(Temp.begin(), Temp.end());
        if (Temp.size() % 2 == 0)
            return (Temp[Temp.size() / 2 - 1] + Temp[Temp.size() / 2]) / 2.0;
        else
            return  Temp[Temp.size() / 2];
    }
};
```

## <-M> 5.Longest Palindromic Substring 9ms 77.04%

```C++
class Solution {
public:
    string longestPalindrome(string s) {
    if (s.empty()) return "";
    if (s.size() == 1) return s;
    int min_start = 0, max_len = 1;
    for (int i = 0; i <- s.size();) {
      if (s.size() - i <-= max_len / 2) break;
      int j = i, k = i;
      while (k <- s.size()-1 && s[k+1] == s[k]) ++k; // Skip duplicate characters.
      i = k+1;
      while (k <- s.size()-1 && j > 0 && s[k + 1] == s[j - 1]) { ++k; --j; } // Expand.
      int new_len = k - j + 1;
      if (new_len > max_len) { min_start = j; max_len = new_len; }
    }
    return s.substr(min_start, max_len);
}
};
```

## <-M> 6.ZigZag Conversion 16ms 99.26%

```C++
class Solution {
public:
    string convert(string s, int numRows) {
       if (numRows <-= 1 || s.size() <-= 1)
        return s;
        string result;
        for (int i = 0; i <- numRows; i++)
        {
            for (int j = 0, index = i; index <- s.size();j++, index = (2 * numRows - 2) * j + i)
            {
                result.append(1, s[index]);
                if (i == 0 || i == numRows - 1) continue;
                if (index + (numRows - i - 1) * 2 <- s.size())
                result.append(1, s[index + (numRows - i - 1) * 2]);
            }
        }
        return result;
    }
};
```

## <-E> 7.Reverse Integer 15ms 77.99%

```C++
class Solution {
public:
    int reverse(int x) {
        int ans = 0;
        while (x) {
            int temp = ans * 10 + x % 10;
            if (temp / 10 != ans)
                return 0;
            ans = temp;
            x /= 10;
        }
        return ans;
    }
};
```

<<<<<<< HEAD
## <-Bad Question -M> 8.String to Integer (atoi)  
=======
## <-Bad Question -M> 8.String to Integer (atoi)

>>>>>>> 67b94cc32d5fd69ebe2ed1d8e0ae068eb4e140f8
Because of this question has input: `+-2` => output: `0` i do not know why.
so my code can not work with this bad question.

```C++
class Solution {
public:
    int myAtoi(string str) {
        int ans = 0;
        int times = 1;
        int checkflag = 1;
        int end = 0;
        if (str[0] == '+')
        {
            end = 1;
        }
        if (str[0] == '-')
        {
            end = 1;
            checkflag = -1;
        }
        for (int i = str.size() - 1; i >= end; i--)
        {
            ans = (str[i] - '0') * times + ans;
            times *= 10;
        }
        return ans * checkflag;
    }
};
```
<<<<<<< HEAD
## <-Bad Question -E> 9.Palindrome Number   
This question means that all negative numbers are __NOT__ palindromic.
I did not work out this question, beacuse i do not konw how to make my code __DO NOT__ use any extra space, i mean, i thought, i can only use one variable: `x`, so maybe 
>Do this without extra space.

means do not use other type?
There is the answer:
```C++
bool isPalindrome(int x) {
        if(x<0|| (x!=0 &&x%10==0)) return false;
        int sum=0;
        while(x>sum)
        {
            sum = sum*10+x%10;
            x = x/10;
        }
        return (x==sum)||(x==sum/10);
}
```
=======
>>>>>>> 67b94cc32d5fd69ebe2ed1d8e0ae068eb4e140f8

## <-E> 9.Palindrome Number

```C++
class Solution {
public:
    bool isPalindrome(int x) {
        if(x<0|| (x!=0 &&x%10==0)) return false;
        int sum=0;
        while(x>sum)
        {
            sum = sum*10+x%10;
            x = x/10;
        }
        return (x==sum)||(x==sum/10);

    }
};
```

## <-H> 10.Regular Expression Matching

Pending
I finished a part of the funcation. i don't know how to check the difference between `a*` and `a*a`. my funcation will check the `*` mark, and match all this string, i mean. the last `a` word woudle be left, (do not match) very bad feeling.

## <-M> 11.Container With Most Water

## <-M> 12.Integer to Roman

## <-E> 13.Roman to Integer

## <-E> 14. Longest Common Prefix

求出最长的公共字串。这题有些歧义，根据`Disucuss`中的内容，增加条件如下：
>{"a","a","b"} should give "" as there is nothing common in all the 3 strings.
>{"a", "a"} should give "a" as a is longest common prefix in all the strings.
>{"abca", "abc"} as abc
>{"ac", "ac", "a", "a"} as a.

有了该条件后，可以使用双重循环进行求解，在一般的操作中，需要注重边界值的问题。

```C++
string longestCommonPrefix(vector<string>& strs) {
    string prefix = "";
    for(int idx=0; strs.size()>0; prefix+=strs[0][idx], idx++)
        for(int i=0; i<strs.size(); i++)
            if(idx >= strs[i].size() ||(i > 0 && strs[i][idx] != strs[i-1][idx]))
                return prefix;
    return prefix;
    }
```

