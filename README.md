# leetcode——Noob的成长之路


## 1. Two Sum
#### * First solution(easy one):
    > 整体思想：通过利用一个空数组将原数组中元素迭代，来验证target与原数组中元素的商是否已存在于另一数组，
    > 如否，则将该元素添加进另一数组。
    > 其中，时间复杂度为O(x^2).空间复杂度为O(1)


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

#### * Second solution(using hash table)
```
d = {}

if len(nums)>1:

for i in enumerate(nums):

    if (target - num) in d :  

        return [d[target-num], i]

    d[num] = i        
```
> **通过哈希查找，牺牲空间复杂度来换取更快的运行速度，其中，时间复杂度为O(n),空间复杂度为O（n）。**

## 2. Add Two Number
    > Use a variable "carry" to express whether the next digit need to plus 1;
    > if or not the adding two number's sum > 10, let val be (sum + carry), which can be used later;
    > Then let carry be val/10, easy to understand that be 1 if val >= 10 while 0 if val<10
    > Then move the pointer forward, actually we want save storage we use as we can, so let's use ? : operater;

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
