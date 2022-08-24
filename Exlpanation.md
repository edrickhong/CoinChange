
## The problem

You are given a set of coins differing values and a target value to reach. You are asked how many combinations are there to reach the given target value. You are allowed to use a given coin an unlimitted ammount of times to reach the target.

For example, if the target is 4 and a coin value of 2 is given, a valid combination would be:

2 + 2 = 4

There are many solutions which are straight forward but we will be focusing on a dynamic programming solution that can be memoized.

Now let the scenario be such that the target value is 8 and the coins be of values 2, 3 and 5. Thus let the set of coins be **C** and the target value be **V**. Let us focus on a singular coin first. Imagine a function **f(i,j)** where **i** is an element of **C** and **j** is the value to reach and the value returned by the function tells us if a combination of **i** can reach the target value **j** where 1 is *true* and 0 is *false*.

eg: Consider f(2,8). This will return 1 for 2 + 2 + 2 + 2 = 8. This doesn't tell us how many combinations there are considering all the coins but let's start by looking at each coin value individually first.

We'll examine this in a table with each *i*  values and *j* values from 0 to *V* (8):

| C/V | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
| - | - | - | - | - | - | - | - | - | - |
| 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| 2 | 1 | 0 | 1 | 0 | 1 | 0 | 1 | 0 | 1 |
| 3 | 1 | 0 | 0 | 1 | 0 | 0 | 1 | 0 | 0 |
| 5 | 1 | 0 | 0 | 0 | 0 | 1 | 0 | 0 | 0 |

We added a row of coin values 0. This will help in forming the algorithm later. Noticed that when *j* = 0, *f* always returns 1. This is because we can always choose no coins to make a value of 0. Each data entry shows whether a combination of the respective coin value (C) can attain the corresponding target value (V). How about when considering two different values of coins?

If we were to consider coins 2 and 3, can we get a value of say, 5? The answer is obviusly yes: 2 + 3 = 5, but how can we use f(i,j) to solve this?

f(3,5) = 0 and f(2,5) = 0 which doesn't tell us much so let's reform the solution.

Let's call this function f(2..3,5) which considers coins 2 and 3. We do know that 3 can fit into the value 5 and its remainder would be 2, thus the function would be:

So f(2..3,5) = f(2..3,5 - 3) = f(2..3,2) or that the return of f(2..3,5) is dependent on the result of f(2..3,2), thus:

f(2..3,5) = f(2..,3,2) = f(2,2) = 1 or it is possible to form 5 with the set 2, 3. To get the number of combinations for two values, we simply ask, is 2 able to for the value by itself previously? Let F be the sum of the set function f from the first element in **C** up to the first parameter in F:

F(3,5) = f(2..3,5) + f(2,5) = f(2,2) + f(2,5) = 1 + 0 = 1

Thus there is only 1 way to form 5 with 2 and 3. So, more generally:
F(i,j) = F(i - 1, j - i) + F(i - 1,j) where i - 1 represents the previous coin value of i.
The general case expands to more functions of F because we need to consider the summations up to the i-1 point for that functions and it eventually terminates in an f function which yields the actual values.

Thus the total number of combinations can be computed with F(5,8). Instead of recursing from the top down, we we can start from the bottom up, f(2,2) + f(2,5) = f().... = F(5,8).
Doing this simply requires us to run f(i,j) = f(i - 1, j - i) + f(i - 1,j) for each value in **C**.

The table below utilizes f(i,j) = f(i, j - 1) + f(i - 1, j) instead because when i < j, these cells are never used and doing so would simplify the algorithm. In such cases where i < j,
f(i,j) = f(i - 1, j). Thus running this algorithm on our table will give us


| C/V | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
| - | - | - | - | - | - | - | - | - | - |
| 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| 2 | 1 | 0 | 1 | 0 | 1 | 0 | 1 | 0 | 1 |
| 3 | 1 | 0 | 1 | 1 | 1 | 1 | 2 | 1 | 1 |
| 5 | 1 | 0 | 1 | 1 | 1 | 2 | 2 | 2 | 3 |


Thus the answer for F(5,8) is 3.
