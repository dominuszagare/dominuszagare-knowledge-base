---
sidebar_position: 1
---
# Binary search tree

A binary search tree is a binary tree where the value of each node is larger or equal to the values in all the nodes in that node's left subtree and is smaller than the values in all the nodes in that node's right subtree.

## Implementation in Rust

```rust
use std::rc::Rc;
use std::cell::RefCell;

#[derive(Debug)]
struct Node {
    value: i32,
    left: Option<Rc<RefCell<Node>>>,
    right: Option<Rc<RefCell<Node>>>,
}

impl Node {
    fn new(value: i32) -> Self {
        Self {
            value,
            left: None,
            right: None,
        }
    }

    fn insert(&mut self, value: i32) {
        if value <= self.value {
            if let Some(left) = &self.left {
                left.borrow_mut().insert(value);
            } else {
                self.left = Some(Rc::new(RefCell::new(Node::new(value))));
            }
        } else {
            if let Some(right) = &self.right {
                right.borrow_mut().insert(value);
            } else {
                self.right = Some(Rc::new(RefCell::new(Node::new(value))));
            }
        }
    }

    fn contains(&self, value: i32) -> bool {
        if value == self.value {
            return true;
        }

        if value < self.value {
            if let Some(left) = &self.left {
                return left.borrow().contains(value);
            }
        } else {
            if let Some(right) = &self.right {
                return right.borrow().contains(value);
            }
        }

        false
    }
}

fn main() {
    let mut root = Node::new(10);
    root.insert(5);
    root.insert(15);
    root.insert(20);
    root.insert(0);
    root.insert(-5);
    root.insert(3);

    println!("{:?}", root);

    println!("{}", root.contains(15));
    println!("{}", root.contains(3));
    println!("{}", root.contains(100));
}
```