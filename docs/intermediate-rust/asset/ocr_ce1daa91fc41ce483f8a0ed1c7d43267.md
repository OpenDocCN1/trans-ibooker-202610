invariant, and contravariant. A type is covariant if you can just use a subtype in place of the type. For example, if a variable is of type &'a T, you can provide a value of type &'static T to it, because &'a T is covariant in 'a. &'a T is also covariant in T, so you can pass a Vec<&'static str> to a function that takes &Vec<&'a str>.

Some types are invariant, which means that you must provide exactly the given type. &mut T is an example of this—if a function takes a &mut Vec<&'a str>, you cannot pass it a &mut Vec<&'static str>. That is, &mut T is invariant in T. If you could, the function could put a short-lived string inside the Vec, which the caller would then continue using, thinking that it were a Vec<&'static str> and thus that the contained string were 'static! Any type that provides mutability is generally invariant for the same reason—for example, Cell<T> is invariant in T.

The last category, contravariance, comes up for function arguments. Function types are more useful if they're okay with their arguments being less useful. This is clearer if you contrast the variance of the argument types on their own with their variance when used as function arguments:

```
let x: &'static str; // more useful, lives longer
let x: &'a str; // less useful, lives shorter

fn take_func1(&'static str) // stricter, so less useful
fn take_func2(&'a str) // less strict, more useful
```

This flipped relationship indicates that Fn(T) is contravariant in T.

So why do you need to learn about variance when it comes to lifetimes? Variance becomes relevant when you consider how generic lifetime parameters interact with the borrow checker. Consider a type like the one shown in Listing 2-11, which uses multiple lifetimes in a single field.

```
struct MutStr<'a, 'b> {
    s: &'a mut &'b str
}
let mut s = "hello";
① *MutStr { s: &mut s }.s = "world";
println!("{}", s);
```

Listing 2-11: A type that needs to be generic over multiple lifetimes

At first glance, using two lifetimes here seems unnecessary—we have no methods that need to differentiate between a borrow of different parts of the structure, as we did with StrSplit in Listing 2-10. But if you replace the two lifetimes here with a single 'a, the code no longer compiles! And it's all because of variance.

At ①, the compiler must determine what lifetime the lifetime parameter(s) should be set to. If there are two lifetimes, 'a is set to the to-be-determined lifetime of the borrow of s, and 'b is set to 'static since that's the lifetime of the provided string "hello". If there is just one lifetime 'a, the compiler infers that that lifetime must be 'static.