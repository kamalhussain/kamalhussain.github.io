---
layout: default
title:  "Loops in recursive functions"
date:   2017-12-04 14:20:20 -0600
categories: programming
---

Do recursive functions scare you? How about recursive functions within a for loop? Don't run away. Let's try to break it down and develop a real sense about how recursive functions work.

Finding fibonacci numbers is a classic case of recursion and the code looks like the following:

```python
def fib(n):
    if n == 0: return 0
    if n == 1: return 1

    return fib(n - 1) + fib(n - 2) 
 ```
 This is very simple form of recursion that is easy to visualize. Let's add some print statements and track the flow of the program.
 
 ```python
def fib(n):
    print("entering %s", (n))
    
    if n == 0: 
      print("returning 0")
      return 0
      
    if n == 1: 
      print("returning 1")
      return 1

    ret = fib(n - 1) + fib(n - 2) 
    print("returning %s" % ret)
 ```
 If you call fib(5), the output would be the following:
 ```
entering 5 
entering 4 
entering 3 
entering 2 
entering 1 
returning 1
entering 0 
returning 0
returning 1 
entering 1 
returning 1
returning 2 
entering 2 
entering 1 
returning 1
entering 0 
returning 0
returning 1 
returning 3 
entering 3 
entering 2 
entering 1 
returning 1
entering 0 
returning 0
returning 1 
entering 1 
returning 1
returning 2 
returning 5
```
As you can see, the program enters the function with values 5, 4, 3, 2, and 1. As soon as it hits 1, the function returns. The same is true for the value 0. These are the basecases of the recursion. Another way to think about recursion is that it is top-down approach, where the higher level problem is broken into smaller sub problems. Every function call such as fib(5) is broken into sub problems of fib(4), fib(3), fib(2), fib(1) and fib(0).

Fair enough. How about a problem where there are more complex sub problems? Let's look at the "Making change" problem that is published at [www.byte-by-byte.com](http://www.byte-by-byte.com/).

Here is the problem description (courtsey: bytebybyte): Given an integer representing a specific amount of change, write a function to compute the total number of coins required to make that amount of change. Example: Assume there is a coinset of  1, 4, 6 and the total amount is 8. You can reach the total amount by using two 1 coins and one 4 coin or using two 4 coins. The correct answer is the latter since it requires fewer number of coins.

The above problem can be solved by using a recursive approach. The recursive function will find every possible combination of coins. We can start with the total amount and then in each recursive step, break it into three sub-problems (for each coin) until the total sum is zero. Here is one implementation of such function:

```python
def makeChange(x, coins):
    if x == 0: return 0
    min = x

    for coin in coins:
        if coin <= x:
            c_min = makeChangeV3(x - coin, coins)
            if (c_min + 1) < min:
                min = c_min + 1
    return min
```
The function can be invoked as makeChange(8, [1, 2, 4]). This function looks simple but there are two aspects that may throw you off. First, there is a variable called "min" defined inside the recursive function. Second, there is a for loop at every recursive step.

Everytime, the recursive function is called, the variable min is re-initialized to the value x. The key is understanding that the variable x is reassigned to c_min later in the program, which ultimately determines what is being returned from the function.

The for loop inside the recursion may be hard to visualize at the beginning. The simplest way to think about this abstraction is that at every stage the current problem is being broken in/branch out to as many sub problems as the number of coins. In this case, number of coin options turned out to be 3.

Unlike the fibonacci example, this function breaks into three different branches at every step. The conditional statement inside the for loop makes sure that each branch terminates at certain point.

Comparing and contrasting fibonacci and make change examples would help you understand different variations in recursion.


