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
### 1029 Two City Scheduling
<span style="border: 1px white;background-color:#F39C12;border-radius: 10px;padding: 5px; color: white; margin:5px">Medium</span><span style="border: 1px white;background-color:#884EA0;border-radius: 10px;padding: 5px; color: white; margin:5px">DP</span>
#### Description
Given a set of **distinct** positive integers, find the largest subset such that every pair $(S_i, S_j)$ of elements in this subset satisfies:

$S_i % S_j = 0 or S_j % S_i = 0$

If there are multiple solutions, return any subset is fine.


#### Example
```
Input: [1,2,3]
Output: [1,2] (of course, [1,3] will also be ok)

Input: [1,2,4,8]
Output: [1,2,4,8]
```
#### Strategy

|Approach|Time Complexity|Space Complexity|
|:--:|:--:|:--:|
|**Greedy**|$O(NlogN)$|$O(N)$|

#### Intuition
We should first sort the given array, so that we could use the mathematical corollaries to simplify the solution.

Then we define a function $EDS(X_i)$, which gives the largest subset that ends with the number $X_i$. E.g., in ${2,4,7,8}$, $EDS(4) = {2,4}$, $EDS(7) = {7}$. Then our target function is $LDS([X_1, X_2, ... X_n])$, now we have 

$$LDS([X_1, X_2, ... X_n]) = max(\forall EDS(X_i)), 1 \leq i \leq n $$

#### Code
The code is self-explanatory and pythonic.
```
class Solution(object):
    def largestDivisibleSubset(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        # The container that holds all intermediate solutions.
        # key: the largest element in a valid subset.
        subsets = {-1: set()}
        
        for num in sorted(nums):
            subsets[num] = max([subsets[k] for k in subsets if num % k == 0], key=len) | {num}
        
        return list(max(subsets.values(), key=len))
```

### 518. Coin Change 2
<span style="border: 1px white;background-color:#F39C12;border-radius: 10px;padding: 5px; color: white; margin:5px">Medium</span><span style="border: 1px white;background-color:#884EA0;border-radius: 10px;padding: 5px; color: white; margin:5px">DP</span><span style="border: 1px white;background-color:#3498DB;border-radius: 10px;padding: 5px; color: white; margin:5px">Search</span>
#### Description
You are given coins of different denominations and a total amount of money. Write a function to compute the number of combinations that make up that amount. You may assume that you have infinite number of each kind of coin.


#### Example 1
```
Input: amount = 5, coins = [1, 2, 5]
Output: 4
Explanation: there are four ways to make up the amount:
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
```
#### Example 2
```
Input: amount = 3, coins = [2]
Output: 0
Explanation: the amount of 3 cannot be made up just with coins of 2.
```
#### Example 3
```
Input: amount = 10, coins = [10] 
Output: 1
```
#### Strategy

Let $M \leftarrow amount$, $N \leftarrow coins$, then
##### Approach 1: Search

|Approach|Time Complexity|Space Complexity|
|:--:|:--:|:--:|
|**DFS**|$O(M^N)$|$O(M^N)$|

##### Intuition
Let's first sort the coins. Then we start from an initial state to search for the whole state. In each round, we update our current states by trying to add each coin into the sum, and we know about at least after $k$ rounds, we could gererate all  possible sums that consistute the amount, where $k = \frac{amount}{min(coins)}$.

##### Code
The code is self-explanatory. I use a dictionary to store all possible states, where key is the sum and value is set of the all feasible combinations.
```
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        if not coins: return int(not amount)
        coins = sorted(coins)
        dic = {0:set([tuple([0] * len(coins))])}
        for i in range(amount // min(coins)):
            newdic = {}
            for k, comps in dic.items():
                for c in comps:
                    for idx, w in enumerate(coins):
                        if w + k > amount: continue
                        newcomp = list(c)
                        newcomp[idx] += 1
                        if w+k not in newdic:
                            newdic[w+k] = set([tuple(newcomp)])
                        else:
                            newdic[w+k].add(tuple(newcomp))
            # update dic
            for k, v in newdic.items():
                if k not in dic: dic[k] = v
                else:   dic[k] |= v 
        return len(dic[amount]) if amount in dic else 0
```
Unfortrunately, this approach would cause TLE, although its result is correct and intuitive and clear to view.

##### Approach 2: DP

|Approach|Time Complexity|Space Complexity|
|:--:|:--:|:--:|
|**DP**|$O(MN)$|$O(M)$|

##### Intuition

Add coins one-by-one, starting from base case 'no coins'.

For each added coin, compute recursively the number of combinations for each amount of money from 0 tot `amount`.

Algo:

- Initiate number of combinations array with the base case 'no coins': `dp[0] = 1` and all the rest = 0.

- Loop over all coins:
    - for each coin, loop over all amounts from 0 to `amount`:
        - for each amount `x`, computer the number of combinations: `dp[x] += dp[x-coin]`.


##### Code
The code is self-explanatory. 
```
class Solution:
    def change(self, amount: int, coins: List[int]) -> int:
        if not coins: return int(not amount)
        dp = [0] * (amount + 1)
        dp[0] = 1
        for coin in coins:
            for x in range(coin, amount+1):
                dp[x] += dp[x-coin]
        return dp[amount]
```
This approach is clear and excellent. One key clue is that we should consider the dp recursion from the point of each coin. Another is that loop coin enumeration first and loop amount secondly.

However, it's not a intuitive dp problem, for the dp recurrence depends on itself and it's hard to complain.


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