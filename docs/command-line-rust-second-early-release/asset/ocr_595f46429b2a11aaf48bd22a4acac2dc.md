> **TIP**
> 
> Rust can prevent you from making all sorts of egregious errors, but it can’t stop you from doing stupid things. There are still plenty of ways for you to shoot yourself in the foot.

Following is perhaps the shortest way to read the desired number of bytes from a file:

```
let bytes: Result<Vec<_, _>> =
file.bytes().take(num_bytes).collect();
print!("{}", String::from_utf8_lossy(&bytes?));
```

In the preceding code, the type annotation `Result<Vec<_, _>>` is necessary as the compiler infers the type of `bytes` as a slice, which has an unknown size. I must indicate I want a `Vec`, which is a smart pointer to heap-allocated memory. The underscores (`_`) here indicate partial type annotation, which basically instructs the compiler to infer the types. Without this, the compiler complains thusly:

```
Compiling headr v0.1.0 (/Users/kyclark/work/sysprog-rust/playground/headr)
error[E0277]: the size for values of type `[u8]` cannot be known at compilation time
  --> src/lib.rs:95:58
   |
95 |                     print!("{}",
   | String::from_utf8_lossy(&bytes?);
   | ^^^^^^^ doesn't
   |                     have a size
known at compile-time
   |
   = help: the trait `Sized` is not implemented for `[u8]`
   = note: all local variables must have a statically known size
   = help: unsized locals are gated as an unstable feature
```