---
layout: post
title: 'LeetCode Problem Solvers'
subtitle: 'Record some interesting leetcode solutions and thoughts.'
date: 2020-06-16
author: Hang Yu
categories: Leetcode
cover: 'https://github.com/kakayuw/kakayuw.github.io/blob/master/assets/img/leetcode.jpg'
tags: Leetcode
---
## Dynamic Programmings
Fantastic DP problems.

----
## Array 
Array related optimization problems.
### 1029 Two City Scheduling
<span style="border: 1px white;background-color:limegreen;border-radius: 10px;padding: 5px; color: white; margin:5px">Easy</span><span style="border: 1px white;background-color:khaki;border-radius: 10px;padding: 5px; color: white; margin:5px">Greedy</span>
#### Description
There are 2N people a company is planning to interview. The cost of flying the i-th person to city A is costs\[i\]\[0\], and the cost of flying the i-th person to city B is costs\[i\]\[1\].

Return the minimum cost to fly every person to a city such that exactly N people arrive in each city.


#### Example
```
Input: [[10,20],[30,200],[400,50],[30,20]]
Output: 110
Explanation: 
The first person goes to city A for a cost of 10.
The second person goes to city A for a cost of 30.
The third person goes to city B for a cost of 50.
The fourth person goes to city B for a cost of 20.

The total minimum cost is 10 + 30 + 50 + 20 = 110 to have half the people interviewing in each city.
```
#### Strategy

|Approach|Time Complexity|Space Complexity|
|:--:|:--:|:--:|
|**Greedy**|$O(NlogN)$|$O(N)$|

#### Intuition
For each person, he has only two choices: sent to city A or city B. Thus, each person could generate a gain - the difference between the two choices. Then in accordance with the greedy strategy, we sort the cost array, pick the local optimal choice until one city cannot accormodate more person. At last we collect the remains not counted in our approach.
#### Code
The code is self-explanatory.
```
class Solution:
    def twoCitySchedCost(self, costs: List[List[int]]) -> int:
        costs = sorted(costs, key=lambda x: abs(x[0]-x[1]), reverse=True)
        numa = numb = len(costs) // 2
        cost = 0
        rest_start = 0
        # max gain for one city
        for idx, (a, b) in enumerate(costs):
            cost += min(a, b)
            if a < b:   numa -= 1
            else:   numb -= 1
            if numa == 0 or numb == 0: 
                rest_start = idx + 1
                break
        # collect the remains for the other
        rest = 0 if numb == 0 else 1
        cost += sum(map(lambda x: x[rest], costs[rest_start:]))
        return cost
```