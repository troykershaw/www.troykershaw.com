+++
title = "Insertion Sort"
date = 2021-06-07

tags = ["rust", "clrs", "algorithm"]
+++

This implementation is pretty much straight out of the [book](https://en.wikipedia.org/wiki/Introduction_to_Algorithms) and we're required to use the `Copy` trait as a result, limiting `T` a great deal.

```rust
fn insertion_sort_with_copy<T: PartialOrd + Copy>(a: &mut [T]) {
    for j in 1..a.len() {
        let key = a[j];
        let mut i = j;
        while i > 0 && a[i - 1] > key {
            a[i] = a[i - 1];
            i -= 1;
        }
        a[i] = key;
    }
}
```

By changing things around a little and using `swap`, we can remove the need for the `Copy` trait.

```rust
fn insertion_sort<T: PartialOrd>(a: &mut [T]) {
    for j in 1..a.len() {
        let mut i = j;
        while i > 0 && a[i - 1] > a[i] {
            a.swap(i - 1, i);
            i -= 1;
        }
    }
}
```

The end!
