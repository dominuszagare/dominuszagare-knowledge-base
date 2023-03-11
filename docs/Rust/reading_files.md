# Reading files
Often you will need to read files in your Rust programs. This is a very common task and there are many ways to do it.

## Links

- [How to read files](https://blog.logrocket.com/how-to-read-files-rust/)

## Finding files in a directory

```rust
use std::fs;

fn main() {
    let paths = fs::read_dir("./").unwrap();

    for path in paths {
        println!("{:?}", path.unwrap().path());
    }
}
```

## Creating a directory tree

```rust
use std::fs;

fn main() {
    fs::create_dir_all("a/b/c").unwrap();
}
```