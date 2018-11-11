---
layout:     post
title:      "940. Distinct Subsequences II"
date:       2018-11-11 14:00:00
author:     "Fan"
catalog: true
tags:
- dp
- dp-1D
- dp-explore
- leetcode-subsequence
- leetcode
---

## Problem Description
Given a string S, count the number of distinct, non-empty subsequences of S .

Since the result may be large, return the answer modulo 10^9 + 7.


```
Example 1:

Input: "abc"
Output: 7
Explanation: The 7 distinct subsequences are "a", "b", "c", "ab", "ac", "bc", and "abc".
Example 2:
```
```
Input: "aba"
Output: 6
Explanation: The 6 distinct subsequences are "a", "b", "ab", "ba", "aa" and "aba".
Example 3:
```
```
Input: "aaa"
Output: 3
Explanation: The 3 distinct subsequences are "a", "aa" and "aaa".

```


Note:

S contains only lowercase letters.
1 <= S.length <= 2000


## 1-D dp O(n)
> Time: O(n), space O(n)

**Without duplicate analysis**:

First let's see without duplicate: `abc`

dp[0] = 1, ` ` |

dp[1] = 2, ` ` | `a`

dp[2] = 4, ` ` | `a` | `b` `ab`

dp[3] = 8, ` ` | `a` | `b` `ab` | `c`  `ac`  `bc` `abc`

We find that we just need to use the `s.charAt(i)` to add up all of the previous results, then we could get the new results. For example, use `c` to add up all results of dp[2].

So the `dp[i] = dp[i - 1] * 2`

**With Duplicate Analysis**

What if we have duplicate: `abca` or `abcb` or `abcc` ?

dp[0] = 1, `` |

dp[1] = 2, '' | 'a'

dp[2] = 4, '' | 'a' | 'b' 'ab'

dp[3] = 8, '' | 'a' | 'b' 'ab' | 'c'  'ac'  'bc' 'abc'

The above results are not change

For `abca`, the only duplicate is `a`. [red means duplicate]

dp[4] = 15, '' | `'a'`| 'b' 'ab' | 'c'  'ac'  'bc' 'abc' |

For `abcb`, the duplicaate include `ab`, `ab`.

dp[4] = 13, '' | 'a' | `'b' 'ab' `| 'c'  'ac'  'bc' 'abc' |

So we could find it is `the last char that is the same as the current char` that cause the duplicate subsequence!!

So we could have:

```
dp[i] = dp[i - 1] * 2 - last[S[i - 1] - 1]

last = new int[26];
*the index of the array is the char 'a', 'b' ... 'z',
*the value is the index of S.
```

***Why last[S[i - 1] - 1]***

* S[i - **1**] is to match the dp to the string index
* S[i - 1] - **1** is because the **current** char and **previous** casue duplicate.

how many duplicates? it is the **previous of previous**dp number.

For example, `abcb` , both two `b` will be  added to dp[1] =  '' , 'a' and get the results ***'b' 'ab'***

dp[0] = 1, '' |

dp[1] = 2, '' | 'a'

dp[2] = 4, '' | 'a' | ***'b' 'ab'***

```java
public int distinctSubseqII(String S) {
        int MOD = 1_000_000_007;
        // the index of the array is the char 'a', 'b' ... 'z',
        // the value is the index of S
        int [] last = new int[26];
        Arrays.fill(last, -1);

        int [] dp = new int[S.length() + 1];
        dp[0] = 1;
        for(int i = 1; i < dp.length; i ++) {
            int c = S.charAt(i - 1) - 'a';
            // last[c] means the last index of char c
            dp[i] = 2 * dp[i - 1] % MOD;
            if(last[c] >= 0) {
                dp[i] -= dp[last[c] - 1]; // be aware minus - 1
            }
            dp[i] = dp[i] % MOD;
            last[c] = i;
        }
        dp[S.length()] --;
        if (dp[S.length()] < 0) {

        }
        return dp[S.length()] < 0 ? dp[S.length()] + MOD : dp[S.length()];
    }
```
