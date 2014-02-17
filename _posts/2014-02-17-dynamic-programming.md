---
layout: post
title: Coding Practice - Dynamic Programming
categories:
- blog
---

Since coming to Japan, I've gotten used to carrying a significant amount of coins.
The smallest banknote we have here is [1000 yen](http://en.wikipedia.org/wiki/1000_yen_note), which is roughly equivalent to 10 Australian dollars.
There used to be a [500 yen note](http://en.wikipedia.org/wiki/Banknotes_of_the_Japanese_yen#1957-69), but it's no longer in circulation.
For denominations less than 1000, there are [coins](http://en.wikipedia.org/wiki/Japanese_yen#Coins): 1, 5, 10, 50, 100 and 500 yen.
Here's a question: 

> If someone asked you to give them 1000 yen in coins, in how many different ways could you do it?

We'll assume you're loaded and have an infinite supply of coins of each type.
For example, if you're mean, you could give them one thousand 1-yen coins.
Or, you could be nice and just give them two 500-yen coins.

There are several ways of examining this problem.
For starters, you could treat it as a [search](http://en.wikipedia.org/wiki/Search_problem): the search tree would start with no coins being selected, and going down each new level would add a single coin to the selection.
When the total value of the selection reaches 1000, then the search has found *a* solution, so it can backtrack and try a different combination of coins.
The branching factor would be equal to the number of coin types (6, in this case) and the maximum depth of the tree would be 1000 (the "mean" scenario above).
The search would terminate when all possible options have been examined.
The final result would be the total number of solutions we encountered.
Since we're not interested in the order of coins within the solution, there will be some duplicates: for example, picking [1, 5] is the same as picking [5,1] - remember, we want [combinations](http://en.wikipedia.org/wiki/Combination), not [permutations](http://en.wikipedia.org/wiki/Permutation).
We should either test solutions for uniqueness prior to counting them, or invent a clever way of performing the search that avoids duplication.
No matter how we put it, this would be a [brute-force search](http://en.wikipedia.org/wiki/Brute-force_search).

There must be a better way.
From the several puzzles I've solved on [Project Euler](http://projecteuler.net), I've learnt that whenever somebody asks you to count something, enumerating that thing one by one is pretty slow.
Not only is it slow, but it's almost never the best way to go.
Skipping the enumeration and going straight to the counting is far quicker.
But how do you do it?

Suppose I gave you a simpler problem: using 1-yen coins only, make 10 yen.
How many ways of doing that are there?
Before you answer that, what if we could use 1- and 5-yen coins to make 10 yen?
Could you somehow use the answer from the first problem to help answer the second?
It turns that that Yes, We Can&trade;. 
Not only that, but we can continue to combine the solutions of larger, non-trivial sub-problems, until we reach our final solution.
This is known as [Dynamic Programming](http://en.wikipedia.org/wiki/Dynamic_programming).
More specifically, the approach above is known as bottom-up dynamic programming, since we're starting with smaller sub-problems and combining their solutions to solve a greater problem.

To see it in action, check out this [JSFiddle](http://jsfiddle.net/gh/gist/jquery/2.1.0/8971200) that I made (also see [gist](https://gist.github.com/mpenkov/8132774) here).
Hat tip to [@jeanlauliac](https://gist.github.com/jeanlauliac/8674996) for writing up a nicely documented JavaScript solution - have a look at his documented source to see how exactly the algorithm works.
If you'd like a summary in layman terms, read on.

The most interesting part of the solution is a 2D matrix.
Each row corresponds to a sub-amount, and each column corresponds to the largest coin you can use.
Each cell of the matrix corresponds to a sub-problem: how many ways can you make the sub-amount, using the coins up to and including the largest coin?
If you don't use the largest coin, then you can find the number of combinations in the cell to the right.
If you *do* use the largest coin, then you can find the number of combinations in one of the rows above (the precise row depends only on the value of the largest coin and the sub-amount).
Adding the number of combinations together gives the complete solution to the sub-problem.
Since the bottom row of the matrix corresponds to the final amount, the last cell of that row will be the final solution.

Oh, and it turns out that there are 248908 ways of making 1000 yen from coins only.
Just out of interest, there are 6148 of making [5 dollars](http://en.wikipedia.org/wiki/Australian_five-dollar_note) (the smallest note in Australia) from coins only.
Finally, if you're in the US, you're lucky: the smallest note is 1 dollar, and with [pennies, nickels, dimes, quarter-dollar, half-dollar and dollar coins](http://en.wikipedia.org/wiki/Coins_of_the_United_States_dollar) (1, 5, 10, 25, 50, 100 cents, respectively), there are only 293 combinations. 
