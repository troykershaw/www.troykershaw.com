+++
title = "Two Sum in Rust"
date = 2020-10-25

tags = ["rust", "leetcode", "problem"]
+++

What a great example of an interview question; there are a few levels of difficulty making the problem accessible to many, a fairly obvious naive O(n<sup>2</sup>) approach with an oportunity for optimisation, and the curious will find that you can do it in O(n) as well. If I ever run another tech interview training course I'll probably break apart and analyse this problem with the class. It is, however, quite easy and not ideal for an _actual_ interview. I've never asked this question in an interview, but I've read some interview feedback where this question was asked and most recent college graduates are able to solve it. It's also the first problem on LeetCode and I expect pretty well known by now. If you happen to be reading my blog because my name is on your interview slate you can stop reading now as I'm not going to ask you to solve it. :)

## The Problem

Given a collection of integers, `nums`, and an integer, `target`, return the indices of two numbers in `nums` such that their sum equals `target`. Each input has only one solution and you may not use the same item twice.

## O(n<sup>2</sup>) approach

The first approach that will likely come to mind is for each number in the array, iterate through the array, add the two numbers and return if it equals the target, but there's an opportunity for a small optimisation; if the two iterators are `i` and `j`, there's no point having `j` look at `i` or anything before as you've already compared those numbers, so `j` can start at the one more than `i`. Additionally, there's no need for `i` to look at the last item in the list.

> Note: I'm using `Vec<i32>` for the parameters and return types because that's what LeetCode expects.

```rust
fn two_sum_naive(nums: Vec<i32>, target: i32) -> Vec<i32> {
    if nums.len() < 2 { panic!("'nums' must have at least two items.") };

    for i in 0..nums.len()-1 {
        for j in i+1..nums.len() {
            if nums[i] + nums[j] == target {
                return vec![i as i32, j as i32]
            }
        }
    }

    panic!("No items sum to the target")
}

#[test]
fn two_sum_naive_test() {
    assert_eq!(two_sum_naive(vec![2, 7, 11, 15], 9), vec![0, 1]);
    assert_eq!(two_sum_naive(vec![2, 5, 3], 8), vec![1, 2]);
    assert_eq!(two_sum_naive(vec![2, 9, 4, 3, -6], -2), vec![2, 4]);
}
```

This will get you the following score from LeetCode:

```text
Runtime: 36 ms, faster than 26.02% of Rust online submissions for Two Sum.
Memory Usage: 2.1 MB, less than 21.68% of Rust online submissions for Two Sum.
```

But as discussed earlier, we can go much faster than that.

## O(n) approach

Given the target, `t`, we're trying to find the index of numbers `a` and `b` such that `a + b = t`. In the approach above, for each `a` we looked at all items after `a` to see if they satisfied `b`; to solve the problem in a single pass, however, we need to look to the past.

Solving for `b`, our simple equation above becomes `b = t - a`, so as we iterate through the list we can store what `b` value we would need to satisfy the equation along with the index of `a`. For each item we look to see if there was already an `a` that was looking for the number we currently have and if we do we get the index of `a` and return. A perfect application for a hash table.

```rust
use std::collections::HashMap;

fn two_sum(nums: Vec<i32>, target: i32) -> Vec<i32> {
    if nums.len() < 2 { panic!("'nums' must have at least two items.") };

    let mut diffs: HashMap<i32, usize> = HashMap::new();

    for (i, n) in nums.iter().enumerate() {
        match diffs.get(&n) {
            Some(&i0) => return vec![i0 as i32, i as i32],
            None => { diffs.insert(&target - n, i); }
        }
    }
    
    panic!("No items sum to the target")
}

#[test]
fn two_sum_test() {
    assert_eq!(two_sum(vec![2, 7, 11, 15], 9), vec![0, 1]);
    assert_eq!(two_sum(vec![2, 5, 3], 8), vec![1, 2]);
    assert_eq!(two_sum(vec![2, 9, 4, 3, -6], -2), vec![2, 4]);
}
```

And LeetCode says:

```text
Runtime: 0 ms, faster than 100.00% of Rust online submissions for Two Sum.
Memory Usage: 2.5 MB, less than 21.68% of Rust online submissions for Two Sum.
```
