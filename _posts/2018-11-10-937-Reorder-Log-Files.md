---
layout:     post
title:      "937. Reorder Log Files"
date:       2018-11-10 12:00:00
author:     "Hux"
catalog: true
tags:
- Java-API
- Java-Comparator
- leetcode
---

## Problems
You have an array of logs.  Each log is a space delimited string of words.

For each log, the first word in each log is an alphanumeric identifier.  Then, either:

Each word after the identifier will consist only of lowercase letters, or;
Each word after the identifier will consist only of digits.
We will call these two varieties of logs letter-logs and digit-logs.  It is guaranteed that each log has at least one word after its identifier.

Reorder the logs so that all of the letter-logs come before any digit-log.  The letter-logs are ordered lexicographically ignoring identifier, with the identifier used in case of ties.  The digit-logs should be put in their original order.

Return the final order of the logs.


Example 1:

```
Input: ["a1 9 2 3 1","g1 act car","zo4 4 7","ab1 off key dog","a8 act zoo"]
Output: ["g1 act car","a8 act zoo","ab1 off key dog","a1 9 2 3 1","zo4 4 7"]
```

Note:

0 <= logs.length <= 100
3 <= logs[i].length <= 100
logs[i] is guaranteed to have an identifier, and a word after the identifier.
Accepted
2,197
Submissions
4,155


## Arrays.sort
String.split(" ", `n`): you could only split to `n` part

Comparator:
* both are not digit --> general compareTo

* 1 is digit && 2 is digit: --> 0

* 1 is digit && 2 is not digit --> 1

* 1 is not digit && 2 is digit --> -1



```java
class Solution {
    public String[] reorderLogFiles(String[] logs) {
        Arrays.sort(logs, (l1, l2) -> {
            String [] sp1 = l1.split(" ", 2);
            String [] sp2 = l2.split(" ", 2);
            boolean isDigit1 = Character.isDigit(sp1[1].charAt(0));
            boolean isDigit2 = Character.isDigit(sp2[1].charAt(0));
            if(!isDigit1 && !isDigit2) {
                return sp1[1].compareTo(sp2[1]);
            }
            // 1 is digit && 2 is digit: --> 0
            // 1 is digit && 2 is not digit --> 1
            // 1 is not digit && 2 is digit --> -1
            return !isDigit1 ? -1 : (isDigit2 ? 0 : 1);
        });
        return logs;
    }
}
```
