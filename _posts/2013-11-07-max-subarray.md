---
layout: post
title: Coding Practice - Maximum Subarray Problem
categories:
- blog
---

As part of my somewhat regular programming practice, I've recently looked at the [maximum subarray problem](http://en.wikipedia.org/wiki/Maximum_subarray_problem).
Basically, the problem goes like this: given an array of $N$ integers, find the greatest sum of contiguous elements.
According to [Wikipedia](http://en.wikipedia.org/wiki/Maximum_subarray_problem), the problem was first posed in 1977, with a solution of $O(N \log N)$.
A linear-time algorithm was proposed soon thereafter, in 1984.
The goal for the practice exercise was to come up with a linear-time algorithm, from scratch.

To get the ball rolling, if often helps to think of sub-optimal solutions.
There's an obvious $O(N\^3)$ solution that's fairly easy to come up with:

{% highlight python %}
    def max_subarray(A):
      N = len(A)
      max_sum = 0
      for start in range(N):
        for end in range(start+1, N):
          current_sum = sum(A[start:end])
          max_sum = max(max_sum, current_sum)
      return max_sum
{% endhighlight %}

Why is it $O(N\^3)$?
Well, the outer loop gets executed for exactly $N$ elements.
The inner loop gets executed for $N-1$ elements the first time, $N-2$ elements the second time, and so on. 
The inner loop also contains a sum that takes linear time to calculate.
The overall complexity of the solution is therefore $O(N \times N \times N)$.

Obviously, we can do better than this.
In particular, instead of calculating the sum for each iteration of the inner loop, we can build it up incrementally.

{% highlight python %}
    def max_subarray(A):
      N = len(A)
      max_sum = 0
      for start in range(N):
        current_sum = 0
        for i in range(start, N):
          current_sum += A[i]
          max_sum = max(max_sum, current_sum)
      return max_sum
{% endhighlight %}

Since we've replaced the linear-time sum calculation with a pair of constant-time operations, the complexity of this solution is now $O(N\^2)$.
It's still not linear, but it's better than what we started with.

So, how about the linear solution?
The solution I first came up with applied the same trick that we used to go from $O(N\^3)$ to $O(N\^2)$ - modify the sum incrementally.
First, build the sum up incrementally - while it's positive, continue adding elements to the end of the subarray, and keep track of the maximum so far.
If the sum becomes negative, then remove elements from the front of the subarray.
Here's the full solution:

<script src="https://gist.github.com/mpenkov/7297594.js?file=mcs.py"></script>

And here's the same thing in JavaScript, so you can test it in your browser:

Enter some integers: 

<input type="text" id="txtInput" value="-1 5 6 -2 20 -50 4"></input>
<button onClick="btnOnClick();">Calculate</button>
<p>
Max consecutive sum is: <span id="divResult"></span>
</p>
<script>
  function maxConsecutiveSum(numbers) {
    var start = 0;
    var end = 1;
    var current_sum = numbers[0];
    var max_sum = 0;

    while (true) {
      if (current_sum > max_sum) {
        max_sum = current_sum;
      }
      if (current_sum < 0) {
        current_sum -= numbers[start++];
        continue;
      }
      if (end == numbers.length) {
        break;
      }
      current_sum += numbers[end++];
    }
    return max_sum;
  }
  function btnOnClick() {
    var txtInput = document.getElementById("txtInput");
    var integers = txtInput.value.split(" ");
    for (var i = 0; i < integers.length; ++i) {
      integers[i] = parseInt(integers[i]);
    }
    var maxSum = maxConsecutiveSum(integers);
    var divResult = document.getElementById("divResult");
    divResult.innerHTML = maxSum;
  }
</script>

In the end, all this is more complicated than it really needs to be.
It turns out the solution is even simpler than that.
It's known as Kadane's Algorithm, and looks like this:

{% highlight python %}
    def max_subarray(A):
        max_ending_here = max_so_far = 0
        for x in A:
            max_ending_here = max(0, max_ending_here + x)
            max_so_far = max(max_so_far, max_ending_here)
        return max_so_far
{% endhighlight %}

The main difference with my solution is what happens once the current sum becomes negative.
My solution shrinks the array one by one, in an attempt to make the sum positive again.
This attempt is in vain, since removing an element from the subarray will only increase the sum if the removed element was negative.
If the element is positive, then removing it will actually *decrease* the sum, which hardly helps.
Kadane's algorithm realizes this and resets the sum to zero, effectively discarding the subarray completely.

For a more detailed write-up, see [Programming Pearls: Algorithm Design Techniques](http://dl.acm.org/citation.cfm?id=381162) by Jon Bentley et al.
The article is more than 30 years old, but worth the read if you've got access to it.
