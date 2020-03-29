---
layout: post
title:  "const generics"
date:   2020-03-29 09:45:38 +0900
categories: Rust
---
C++ users can write codes like this way.

```c++
template<class T, int length>
struct my_vec {
    std::array<T, length> inner_data;
    ...
};

int main() {
    my_vec<float, 42> some_vec;
}
```

The `inner_data` array size of `my_vec` can be determined by the parametric way using the `length` generic parameter.

In Rust, though there are still some issues to be addressed, Nightly Rust compiler is providing this ability.

The above c++ code can be translated to Rust like this.

```rust
#![feature(const_generics)]
struct MyVec<T: Sized, const LENGTH: usize> {
    inner_data: [T; LENGTH],
}

const A_LEN: usize = 42;

impl<T, const L: usize> MyVec<T, L> {
    pub fn new(value: T) -> Self {
        MyVec {
            inner_data: [value; L],
        }
    }
}

fn main() {
    let _my_vec = MyVec::<f64, 10>::new(4.2);
}
```

Rust compiler would emit this error messages.

```text
error: array lengths can't depend on generic parameters
  --> src/main.rs:11:33
   |
11 |             inner_data: [value; L],
   |
```

For now, `rustc` still has problems to compile this, I wish the `rust team` would solve it in soon.

You can see the [tracking](https://github.com/rust-lang/rust/issues/43408) of this issue.

Thanks for reading.
