---
layout:     post
title:      "115. Distinct Subsequences"
date:       2018-11-11 13:00:00
author:     "Fan"
catalog: true
tags:
- dp
- dp-choose-or-not
- dp-two-strings
- leetcode-subsequence
- leetcode
---

## Problem 115. Distinct Subsequences
Given a string S and a string T, count the number of distinct subsequences of S which equals T.

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, "ACE" is a subsequence of "ABCDE" while "AEC" is not).
```
Example 1:

Input: S = "rabbbit", T = "rabbit"
Output: 3
Explanation:

As shown below, there are 3 ways you can generate "rabbit" from S.
(The caret symbol ^ means the chosen letters)

rabbbit
^^^^ ^^
rabbbit
^^ ^^^^
rabbbit
^^^ ^^^
```

```
Example 2:

Input: S = "babgbag", T = "bag"
Output: 5
Explanation:

As shown below, there are 5 ways you can generate "bag" from S.
(The caret symbol ^ means the chosen letters)

babgbag
^^ ^
babgbag
^^    ^
babgbag
^    ^^
babgbag
^  ^^
babgbag
^^^
```

## Choose or not dp: O(n^2)
> Time O(n^2); Space O(n^2)

For `T` rabbit, we start from len = 0 (`empty`, `r`, `ra` ... `rabbit`).

For `T.len = 0,` the solution is ***aways 1*** because we just don't choose any char from `S` and we could reach to an empty `T`.

Then for T = `r`, we could choose S = `r`, then dp[i][j] = dp[i - 1][j - 1],
or we don't choose S =`r`,  then dp[i][j] = dp[i - 1][j] since it is the same as S is empty and the solution is 0.

Of course, if `s.charAt(i - 1) != t.charAt(j - 1)`, we don't have choise from dp[i - 1][j - 1]

```
dp[i][j] represent how many solutions of t.substring(0, j) and s.substring(0, i)
dp[i][0] = 1,
dp[i][j] = dp[i - 1][j - 1] + dp[i- 1][j],
if s.charAt(i - 1) == t.charAt(j - 1)

dp[i][j] = dp[i - 1][j],  s.charAt(i - 1) != t.charAt(j - 1)
```

```
   '' r  a  b   b   i   t
'' 1  0  0  0   0   0   0
r  1  1  0  0   0   0   0
a  1  1  1  0   0   0   0
b  1  ....
b  1
b  1
i  1
t  1
```

```java
class Solution {
    public int numDistinct(String s, String t) {

        int [][] dp = new int[s.length() + 1][t.length() + 1];
        for(int i = 0; i < dp.length; i ++) {
            dp[i][0] = 1;
        }
        for(int i = 1; i < dp.length; i ++) {
            for(int j = 1; j < dp[0].length; j ++) {
                if(s.charAt(i - 1) == t.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1] + dp[i - 1 ][j];
                } else {
                    dp[i][j] = dp[i - 1][j];
                }
            }
        }
        return dp[s.length()][t.length()];
    }
}
```

