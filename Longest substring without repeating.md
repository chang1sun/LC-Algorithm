# Longest substring without repeating

### 暴力法
此题解决方法用暴力法的思路很简单，两次循环，再将已遍历元素push进数组，判等时迭代，从而再一遍循环，所以时间复杂度为O($n^3$);
### sliding window法
考虑到利用hashmap，只需一次循环，便可找出最长字串，即从i=0，每当遍历一个字符，判断它是否在hash表内，只需常数时间的复杂度，接下来：

如果无：
接着向后推j，i的位置不变

如果有：
则取max值为max(i-j, max),向后推i, j的位置不变.
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




