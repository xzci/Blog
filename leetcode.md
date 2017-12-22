## the first time --
## <-E> 1.Two Sum 286ms 2.67%
```C++
class Solution {
public:
	vector<-int> twoSum(vector<-int>& nums, int target) {
		vector <-int> answer;
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
};
```

## <-M> 2.Add Two Numbers 56ms 27.5%
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

## <-Bad Question - E> 8.String to Integer (atoi)  
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
## <-E> 9.Palindrome Number   

## <-H> 10.Regular Expression Matching   

## <-M> 11.Container With Most Water 

## <-M> 12.Integer to Roman 

## <-E> 13.Roman to Integer   