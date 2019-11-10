# leetcode——Noob的成长之路


## 1. Two Sum
#### First solution(easy one):
> 整体思想：通过利用一个空数组将原数组中元素迭代，来验证target与原数组中元素的商是否已存在于另一数组，
> 如否，则将该元素添加进另一数组。
> 其中，时间复杂度为O(x^2).空间复杂度为O(1)。


```
class Solution(object):

    def twoSum(self, nums, target):

        if len(nums) < 2:

            pass

        r = []    

        for x in range(len(nums)): 

            minus = target - nums[x]

            if minus in r:

                return [x, nums.index(minus)]

            r.append(nums[x])

```

#### Second solution(using hash table)
```
d = {}

if len(nums)>1:

for i in enumerate(nums):

    if (target - num) in d :  

        return [d[target-num], i]

    d[num] = i        
```
> **通过哈希查找，牺牲空间复杂度来换取更快的运行速度，其中，时间复杂度为O(n),空间复杂度为O（n）。**

---

## 2. Add Two Number
> **Use a variable "carry" to express whether the next digit need to plus 1;**

> **if or not the adding two number's sum > 10, let val be (sum + carry), which can be used later;**

> **Then let carry be val/10, easy to understand that be 1 if val >= 10 while 0 if val<10;**

> **Then move the pointer forward, actually we want save storage we use as we can, so let's use ? : operater;**

 *Here is the code:*

```
class Solution {

public:

    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {

        ListNode* head = new ListNode(0);

        ListNode* parent = head; // build a head node;

        int carry = 0;

        while(l1||l2||carry){ //as if  only one condition exist, loop will not stop!

            int val;

            val = (l1?l1->val:0)+(l2?l2->val:0)+carry;

            l1 = l1?l1->next:nullptr;// let it be non-pointer does not harm,so why not?

            l2 = l2?l2->next:nullptr;

            carry = val/10;

            parent->next = new ListNode(val%10); // insert to tail;

            parent = parent->next;

        }

        return head->next;

    }

};
```
---

## 3. Find Common Characters

> ***From one awesome solution, only 1 line python got the deal.***

```

from collections import Counter

from functools import reduce



class Solution:

    def commonChars(self, A: List[str]) -> List[str]:

        return list(reduce(Counter.__and__, map(Counter, A)).elements())

```

> *What have I learned?*

   > 1. **Collection库中Counter函数的实际运用**

   > 2. **reduce应用在字典上，本来的函数应用累加效果变成了在相同key上对value的函数应用累加.**

   > 3. **位运算的实际运用。**



### 基本思路版本：

```
def commonChars(self, A):

        res = collections.Counter(A[0])

        for a in A:

            res &= collections.Counter(a)

        return list(res.elements())
```
---

## 3. Reveal Cards In Increasing Order
*It's disappointed that I have no good idea about manage it in a way that has a less time complexible than O(n!)*

*I try to use the array's method slice, but it proves wrong until I try every way I can imagine. Because it's difficult to* 

*make the sort that one small one big and they have to be sorted in reversed way. Well, let's check a terrific code:*


> **We simulate the reversed process.**

> **Initial an empty list or deque or queue,**

> **each time rotate the last element to the first,**

> **and append a the next biggest number to the left.**



### Time complexity:  O(NlogN) to sort, O(N) to construct using deque or queue.


   ### 1.Python, using list, O(N^2):
```
    def deckRevealedIncreasing(self, deck):

        d = []

        for x in sorted(deck)[::-1]:

            d = [x] + d[-1:] + d[:-1]

        return d
```
        

   ### 2.Python, using deque:

```
    def deckRevealedIncreasing(self, deck):

        d = collections.deque()

        for x in sorted(deck)[::-1]:

            d.rotate()

            d.appendleft(x)

        return list(d)

  ```      
***Anyway, when you got no idea, just might as well read the qurrys itself, then you will find some particular methods.***      

---

## 4. Sort Array By Parity

### *MY first vision:


class Solution(object):

```
def sortArrayByParity(self, A):

        new = []

        for i in A:

            if i % 2 == 0:

                new.append(i)

                A.remove(i)

        new.extend(A)

        return new

```     

> *A awesome vision which beats 99% from comment:*
```

class Solution:

    def sortArrayByParity(self, A):

        """

        :type A: List[int]

        :rtype: List[int]

        """

        start, end = 0, len(A) - 1

        while start < end:

            m, n = A[start], A[end]

            if m % 2 == 1 and n % 2 == 0:   

                A[start], A[end] = n, m     # swap odd and even

            elif m % 2 == 1:                #if end is not even

                end -= 1

            elif n % 2 == 0:                #if start is not odd

                start += 1

            else:

                start += 1

                end -= 1

        return A

 ```         

> *Here is a same idea but shorter:*


```
class Solution(object):

    def sortArrayByParity(self, A):

        """

        :type A: List[int]

        :rtype: List[int]

        """

        l, r = 0, len(A) - 1

        while l < r:            

            if A[l] & 1:                  #位运算与（一个判断奇偶性的不错的方法！）

                A[l], A[r] = A[r], A[l]

            l += not (A[l] & 1)

            r -= A[r] & 1

        return A

```

---

## 5. Longest substring without repeating

### 暴力法
> *此题解决方法用暴力法的思路很简单，两次循环，再将已遍历元素push进数组，判等时迭代，从而再一遍循环，所以时间复杂度为O(n^3);*
### sliding window法
> *考虑到利用hashmap，只需一次循环，便可找出最长字串，即从i=0，每当遍历一个字符，判断它是否在hash表内，只需常数时间的复杂度，接下来:*

> 如果无：
> **接着向后推j，i的位置不变**

> 如果有：
> **则取max值为max(i-j, max),向后推i, j的位置不变.**
##### 时间复杂度为O（2n）即O（n）
### 优化版的sliding window
注意到当第j在hashmap中与第J个元素判等时，可断定J之前，i之后的元素统统不必再循环，因为总会经过J这个字符，且长度逐减
##### 代码如下

```
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length(), ans = 0;
        Map<Character, Integer> map = new HashMap<>(); // current index of character
        // try to extend the range [i, j]
        for (int j = 0, i = 0; j < n; j++) {
            if (map.containsKey(s.charAt(j))) {
                i = Math.max(map.get(s.charAt(j)), i);
            }
            ans = Math.max(ans, j - i + 1);
            map.put(s.charAt(j), j + 1);
        }
        return ans;
    }
}
```

---

## 6. Longest palindromic Substring
*Assume it's a dynamic programing problem. What we need to solve is find what changed and what remaind the same within solving process;*
#### Noticed the palindromic substring:
*It means we just need to set a iterator from 0 to the end of the String; Then set **two pointer which moves forward and backward** to check if the characters are same;*

### *Here is the code:*
```
class Solution:
    def longestPalindrome(self, s: str) -> str:
        maxstr = "", ls = len(s)
        if (len(s) == 1) or (len(s) == 0):
            return s
        for i in range(ls-1):
            if i>0 and (s[i-1] == s[i+1]):
                j,k = i+1,i-1
                while k>=0 and j<ls and(s[k]==s[j]):
                    if len(maxstr) < len(s[k:j+1]):
                        maxstr = s[k:j+1]
                    k-=1
                    j+=1
            if s[i] == s[i+1]:
                j, k = i+1, i
                while k>=0 and j<ls and (s[k]==s[j]):
                    if len(maxstr) < len(s[k:j+1]):
                        maxstr = s[k:j+1]
                    k-=1
                    j+=1
            if (s[i] != s[i+1]) and (i>0 and (s[i-1] != s[i+1])):
                i+=1
        if maxstr == "":
            return s[0]
        else:
            return maxstr
```

---

## Three Sum
*This problem costs me a lot time, just because there is a pile of check point which needs you to debug and go on and go on...*
> **Actually it's not a difficult problem associate with array, and O(n^2) is the best solution in my view;**
#### key: Sorted Array, Two Pointer
> ***First of all, let's sort the array;
> Use i to be the iterator, j and k be two pointers, let nums[i]+nums[j]+nums[k] be target and check its pos or neg, 
> by which to move one of the two pointers forward or backward***

#### *here is my python code*:
```
class Solution:
    def threeSum(self, nums):
        nums = sorted(nums)
        ls = len(nums)
        if ls < 3:
            return
        ans = []
        for i in range(ls):
            j = i+1
            k = ls - 1
            if nums[i]>0:
                break
            if i>0 and nums[i] == nums[i-1]:
                continue
            while j<k:
                target = nums[i]+nums[j]+nums[k]
                if target < 0:
                    j+=1
                elif target > 0:
                    k-=1
                else:
                    ans.append([nums[i],nums[j],nums[k]])
                    while j < k and nums[j] == nums[j+1]:
                        j+=1
                    while k > j and nums[k] == nums[k-1]:
                        k-=1
                    j+=1
                    k-=1
        return ans
```
---

## Median of Two Sorted Array
### Time complexity requirement: **O(log(m+n))**
> ***Note the limit on time complexity, we can't just combine two array and sorted it later, because it takes O(nlog(n)) time complexity;***
***So let's try binary way to solve this, set the two array A and B.***

### 1. First of all, the condition that a median must be satisfied in this problem only contains two limitation:
       1. *set two pointer: i for A, j for B; Then we must find the most fitted i that A[i] is most closed to B[j].*
       2. *i for the A plus j for the B must equal the halflength(when len(A)+len(B) is odd)*
### 2. How to control j by i's change?
    > *use i + j = halflen, so j = halflen - i*
   
#### *Here is the code:*
···
class Solution:
    def findMedianSortedArrays(self,A, B):
        m, n = len(A), len(B)
        if m > n:
            A, B, m, n = B, A, n, m
        if n == 0:
            raise ValueError

        imin, imax, half_len = 0, m, (m + n + 1) / 2
        while imin <= imax:
            i = (imin + imax) / 2
            j = half_len - i
            if i < m and B[j-1] > A[i]:
                # i is too small, must increase it
                imin = i + 1
            elif i > 0 and A[i-1] > B[j]:
                # i is too big, must decrease it
                imax = i - 1
            else:
                # i is perfect

                if i == 0: max_of_left = B[j-1]
                elif j == 0: max_of_left = A[i-1]
                else: max_of_left = max(A[i-1], B[j-1])

                if (m + n) % 2 == 1:
                    return max_of_left

                if i == m: min_of_right = B[j]
                elif j == n: min_of_right = A[i]
                else: min_of_right = min(A[i], B[j])
  
                return (max_of_left + min_of_right) / 2.0

···

---

      
