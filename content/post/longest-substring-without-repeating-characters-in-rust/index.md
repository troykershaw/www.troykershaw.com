+++
title = "Longest Substring Without Repeating Characters in Rust"
summary = ""
date = 2020-10-25

tags = ["rust", "leetcode", "problem"]
+++

Another nice little LeetCode problem.

<!--more-->

## Problem

Given a string, find the length of the longest substring without repeating characters.

Input | Output | Longest substring
--- | --- | ---
`"abcdabcbd"` | 4 | `"abcd"`
`"aaaaaa"` | 1 | `"a"`
`"tidddosa"` | 4 | `"dosa"`
`"au"` | 2 | `"au"`

## Solution

I solved this one by using a sliding window and a set to remember characters I've seen.

```rust
use std::collections::HashSet;
use std::cmp::max;

fn length_of_longest_substring(s: String) -> i32 {
    let chars: Vec<char> = s.chars().collect();

    if chars.len() < 2 { return chars.len() as i32}

    let mut l: usize = 0;
    let mut r: usize = 0;
    let mut longest: usize = 0;
    let mut used: HashSet<char> = HashSet::new();

    while r < chars.len() {
        if used.contains(&chars[r]) {
            used.remove(&chars[l]);
            l += 1;
        }
        else {
            longest = max(longest, r - l);
            used.insert(chars[r]);
            r += 1;
        }
    }

    (longest + 1) as i32
}

#[test]
fn length_of_longest_substring_test() {
    assert_eq!(length_of_longest_substring("".to_string()), 0);
    assert_eq!(length_of_longest_substring("abcdabcbd".to_string()), 4);
    assert_eq!(length_of_longest_substring("aaaaaa".to_string()), 1);
    assert_eq!(length_of_longest_substring("tidddosa".to_string()), 4);
    assert_eq!(length_of_longest_substring("au".to_string()), 2);
}
```

## LeetCode says

```text
Runtime: 4 ms, faster than 76.05% of Rust online submissions for Longest Substring Without Repeating Characters.
Memory Usage: 2.1 MB, less than 100.00% of Rust online submissions for Longest Substring Without Repeating Characters.
```

Not the fastest implementation, but I think it's easy to understand. I'd be interested to see what others have done to make it faster, so ping me on social media if you've got a faster solution.