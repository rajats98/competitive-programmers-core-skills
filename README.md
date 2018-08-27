# Competitive Programmer's Core Skills 

These are my solutions for the course [Competitive Programmer's Core Skills](https://www.coursera.org/learn/competitive-programming-core-skills/), available on Coursera for 59 USD. The problems can be found [in this very repo](./problem_statements.pdf).

## Week 1 (3/4 complete)

### 1.1 Addition and Subtraction

This problem admits an O(1) solution, but I'm stuck. Lots of corner cases; my solution fails on some test cases.

### 1.2 Erasing Maximum

Trivial, runs in O(n) time. 

Run through the array once to get the maximum element. Then run through the
array again and delete either the only maximum element or the third.

### 1.3 Increment

Trivial O(n) solution. We note that unless the number is made of all 9s, incrementing it will not change the number of decimal digits.

### 1.4 Straight Flush

No tricks here, the only hiccup is the Ace. The check for the Ace is not so straightforward. I initialised the Ace as 14 (but it also can act as a 1). So the following code is needed to "convert" the Ace over.

```cpp
int cards[5] = ....;
if (cards[0] == 2 && cards[4] == 14) 
{
    // We have a straight here...
    cards[4] = 1;
    sort(cards);
}
```

Runs in constant time, obviously.

## Week 2 (2/4 complete)

### 2.1 The Cheapest Permutation
My solution runs in 2^n time, which is bad, but `n <= 9`, so this is fine.

I can't find a polynomial time algorithm for this, just used brute force.

For every possible permutation (there are `n!` of them), add them up and get the minimum cost one.

### 2.2 The King

Not sure how to do this one. 

### 2.3 Sum of Minimums

O(n^2) solution, once again by enumerating all the possibilities and memoizing. I hesitate to call this a "dynamic programming" algorithm, because the recurrence relation is trivial.

Initialise a 2-dimensional `n * n` array.
For each value in the input, `a[i][i] = val`.

This gives us the "edge of the pyramid". Then we just fill up the rest according to the recurrence relation.

The recurrence relation is as follows: for an interval `[l, r]`,
`min[l,r] = min( [l+1, r], [l, r-1] )`

Finally, just sum up the whole pyramid.

### 2.4 Expression Evaluation

An obvious and rather trivial O(n) solution presents itself.

## Week 3 (2/4 complete)

### 3.1 Compare Sums

The trick is to realise that the floating point numbers have *exactly 3 digits after the decimal point*. That means that multiplying each floating point number by 1000 gives an integer. Doing this requires some string processing; I used `tmp.substr()` and `tmp.find('.')` to grab the integer bit and the decimal bit.

```cpp
int integer_portion = std::stoi(tmp.substr(0, tmp.find(delimiter))) * 1000;
int decimal_portion = std::stoi(tmp.substr(tmp.find(delimiter) + 1, tmp.size()));
```

and once everything becomes an integer, checking if they're equal is trivial.

### 3.2 Round Up

### 3.3 Yet Another Sum

The problem here is to keep the error small.

I separate the sum and the reciprocal sum. The sum is all integers so there
will be no errors there. Then summing the reciprocal keeps the errors small as the numbers are all <1.

Small gotcha: to do the reciprocal, we can't do `1/n` as this will perform integer division. Instead do `1.0/n` to cast to double.

### 3.4 Binary Knapsack

We know that an exact algorithm for Knapsack is NP-complete (polynomial in the weight `W` of the knapsack). Given that our weight is 10^9, this is rather bad; we need to make use of the binary property of the weights.

Recall that a greedy approach takign best ratio is not optimal. Consider the following knapsack instance:

```
3 4
1 3
2 2
4 6
```
Here the greedy approach taking `1` breaks down as taking `4` is the best.

What's important about the binary property of the weights?

For a good with value `v` and weight `w`, we know right away that 
