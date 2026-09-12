# LeetCode 188 - Best Time to Buy and Sell Stock IV

## Problem

You are given an array `prices` where `prices[i]` represents the price of a stock on the `i`th day.

You are also given an integer `k`, which represents the maximum number of transactions that can be made.

A transaction consists of:

1. Buying one stock.
2. Selling that stock later.

You cannot hold multiple stocks at the same time.

The goal is to find the maximum profit that can be achieved using at most `k` transactions.

## Example

### Input

```text
k = 2
prices = [2,4,1]
```

### Output

```text
2
```

The best transaction is:

```text
Buy at 2
Sell at 4
Profit = 2
```

## Approach

This problem can be solved using **Dynamic Programming**.

We maintain two arrays:

* `buy[j]` → maximum profit after buying while using at most `j` transactions.
* `sell[j]` → maximum profit after selling while using at most `j` transactions.

For every stock price, we update these states.

### Buy State

```python
buy[j] = max(buy[j], sell[j - 1] - price)
```

This means we can either:

* Keep the previous buying state, or
* Buy the stock using the profit obtained after the previous transaction.

### Sell State

```python
sell[j] = max(sell[j], buy[j] + price)
```

This means we can either:

* Keep the previous selling state, or
* Sell the stock and obtain the current price.

## Important Optimization

If:

```text
k >= n / 2
```

then the number of allowed transactions is large enough that we can make as many profitable transactions as possible.

In that case, the problem becomes similar to **Best Time to Buy and Sell Stock II**.

We simply add every positive price difference.

## Python Program

```python
class Solution:
    def maxProfit(self, k, prices):
        n = len(prices)

        if n == 0:
            return 0

        if k >= n // 2:
            profit = 0

            for i in range(1, n):
                if prices[i] > prices[i - 1]:
                    profit += prices[i] - prices[i - 1]

            return profit

        buy = [-float('inf')] * (k + 1)
        sell = [0] * (k + 1)

        for price in prices:
            for j in range(1, k + 1):
                buy[j] = max(buy[j], sell[j - 1] - price)
                sell[j] = max(sell[j], buy[j] + price)

        return sell[k]
```

## Example Explanation

Consider:

```text
k = 2
prices = [3,2,6,5,0,3]
```

We can make at most two transactions.

### Transaction 1

```text
Buy at 2
Sell at 6

Profit = 4
```

### Transaction 2

```text
Buy at 0
Sell at 3

Profit = 3
```

### Total Profit

```text
4 + 3 = 7
```

Therefore:

```text
Output = 7
```

## Key Concept

The main concept used in this problem is **Dynamic Programming**.

We track the best possible profit for different numbers of transactions.

The two important states are:

```text
buy → maximum profit after buying

sell → maximum profit after selling
```

## Why Dynamic Programming?

At every price, we have two main choices:

* Buy the stock.
* Sell the stock.

Instead of checking every possible combination of transactions, we store the best result for each transaction count.

This avoids unnecessary repeated calculations.

## Time Complexity

**O(n × k)**

Where:

* `n` = number of days
* `k` = maximum number of transactions

For every price, we update the states for each transaction count.

## Space Complexity

**O(k)**

Only the `buy` and `sell` arrays of size `k + 1` are maintained.

## Difficulty

**Hard**

## Topics

* Dynamic Programming
* Arrays
* Stock Trading
* State Optimization
* Greedy Optimization

## What I Learned

This problem helped me understand how dynamic programming can be used when there is a limit on the number of transactions.

The important idea is to keep track of the best profit after buying and selling for each possible transaction count.

The main states are:

```text
buy[j]
sell[j]
```

These states are updated for every stock price.

## Author

T.Nandhini
