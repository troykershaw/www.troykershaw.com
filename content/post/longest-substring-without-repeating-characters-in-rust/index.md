+++
title = "Longest Substring Without Repeating Characters in Rust"
summary = ""
date = 2020-10-26

tags = ["rust", "leetcode", "problem"]
+++

Given a string, find the length of the longest substring without repeating characters.

<!--more-->

Input | Output | Longest substring
--- | --- | ---
`"abcdabcbd"` | 4 | `"abcd"`
`"aaaaaa"` | 1 | `"a"`
`"tidddosa"` | 4 | `"dosa"`
`"au"` | 2 | `"au"`

## Solution

We can solve this in O(n) using a sliding window. The `l` and `r` are the left and right indices of the window, respectively. We start with `l` and `r` at 0, representing a window one item wide and then move `r` to the right item by item. We check the set to see if we've seen the item before and if so we get the size of the window (`r - l`) and update `longest` if applicable. When we find an item we've already seen we remove the character at position `l` and move it the right, continuing until we remove the item at `r` from the set. We then start again.

```rust
use std::collections::HashSet;
use std::cmp::max;

fn length_of_longest_substring(s: String) -> i32 {
    let chars: Vec<char> = s.chars().collect();

    if chars.len() < 2 { return chars.len() as i32}

    let mut l: usize = 0;
    let mut r: usize = 0;
    let mut longest: usize = 0;

    // We're dealing with the lowercase English alphabet so we can allocate
    // our set to the max size we'll ever need.
    let mut seen: HashSet<char> = HashSet::with_capacity(26);

    while r < chars.len() {
        if seen.contains(&chars[r]) {
            seen.remove(&chars[l]);
            l += 1;
        }
        else {
            longest = max(longest, r - l);
            seen.insert(chars[r]);
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

## LeetCode says...

```text
Runtime: 4 ms, faster than 76.05% of Rust online submissions for Longest Substring Without Repeating Characters.
Memory Usage: 2.1 MB, less than 100.00% of Rust online submissions for Longest Substring Without Repeating Characters.
```

Not the fastest implementation, but I think it's easy to understand. I'd be interested to see what others have done to make it faster, so ping me on social media if you've got a faster solution.