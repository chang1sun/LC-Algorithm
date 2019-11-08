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





      
