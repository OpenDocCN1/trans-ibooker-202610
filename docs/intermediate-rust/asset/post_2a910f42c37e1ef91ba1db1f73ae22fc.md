
naively, this would take a while to yield results: if the fuzzer starts with a, then b, then c, and so on, it will take it a long time to generate a tricky URL like http://[:]. In practice, modern fuzzers use code coverage metrics to explore different paths in your code, which lets them reach higher degrees of coverage faster than if the inputs were truly chosen at random.

Fuzzers are great at finding strange corner cases that your code doesn’t handle correctly. They require little setup on your part: you just point the fuzzer at a function that takes a “fuzzable” input, and off it goes. For example, Listing 7-7 shows an example of how you might fuzz-test a URL parser.

```
libfuzzer_sys::fuzz_target!(|data: &[u8]| {
    if let Ok(s) = std::str::from_utf8(data) {
        let _ = url::Url::parse(s);
    }
});
```

Listing 7-7: Fuzzing a URL parser with libfuzzer

The fuzzer will generate semi-random inputs to the closure, and any that form valid UTF-8 strings will be passed to the parser. Notice that the code here doesn’t check whether the parsing succeeds or fails—instead, it’s looking for cases where the parser panics or otherwise crashes due to internal invariants that are violated.

The fuzzer keeps running until you terminate it, so most fuzzing tools come with a built-in mechanism to stop after a certain number of test cases have been explored. If your input isn’t a trivially fuzzable type—something like a hash table—you can usually use a crate like arbitrary to turn the byte string that the fuzzer generates into a more complex Rust type. It feels like magic, but under the hood it’s actually implemented in a very straightforward fashion. The crate defines an Arbitrary trait with a single method, arbitrary, that constructs the implementing type from a source of random bytes. Primitive types like u32 or bool read the necessary number of bytes from that input to construct a valid instance of themselves, whereas more complex types like HashMap or BTreeSet produce one number from the input to dictate their length and then call Arbitrary that number of times on their inner types. There’s even an attribute, #[derive(Arbitrary)], that implements Arbitrary by just calling arbitrary on each contained type! To explore fuzzing further, I recommend starting with cargo-fuzz.

## Property-Based Testing

Sometimes you want to check not only that your program doesn’t crash but also that it does what it’s expected to do. It’s great that your add function didn’t panic, but if it tells you that the result of add(1, 4) is 68, it’s probably still wrong. This is where property-based testing comes into play; you describe a number of properties your code should uphold, and then the property testing framework generates inputs and checks that those properties indeed hold.

A common way to use property-based testing is to first write a simple but naive version of the code you want to test that you are confident is correct. Then, for a given input, you give that input to both the code you want to test and the simplified but naive version. If the result or output of the two implementations is the same, your code is good—that is the correctness property you’re looking for—but if it’s not, you’ve likely found a bug. You can also use property-based testing to check for properties not directly related to correctness, such as whether operations take strictly less time for one implementation than another. The common principle is that you want any difference in outcome between the real and test versions to be informative and actionable so that every failure allows you to make improvements. The naive implementation might be one from the standard library that you’re trying to replace or augment (like std::collections::VecDeque), or it might be a simpler version of an algorithm that you’re trying optimize (like naive versus optimized matrix multiplication).

If this approach of generating inputs until some condition is met sounds a lot like fuzzing, that’s because it is—smarter people than I have argued that fuzzing is “just” property-based testing where the property you’re testing for is “it doesn’t crash.”

One downside of property-based testing is that it relies more heavily on the provided descriptions of the inputs. Whereas fuzzing will keep trying all possible inputs, property testing tends to be guided by developer annotations like “a number between 0 and 64” or “a string that contains three commas.” This allows property testing to more quickly reach cases that fuzzers may take a long time to encounter randomly, but it does require manual work and may miss important but niche buggy inputs. As fuzzers and property testers grow closer, however, fuzzers are starting to gain this kind of constraint-based searching capability as well.

If you’re curious about property-based test generation, I recommend starting with the proptest crate.

## TESTING SEQUENCES OF OPERATIONS

Since fuzzers and property testers allow you to generate arbitrary Rust types, you aren’t limited to testing a single function call in your crate. For example, say you want to test that some type Foo behaves correctly if you perform a particular sequence of operations on it. You could define an enum Operation that lists operations, and make your test function take a Vec<Operation>. Then you could instantiate a Foo and perform each operation on that Foo, one after the other. Most testers have support for minimizing inputs, so they will even search for the smallest sequence of operations that still violates a property if a property-violating input is found!

## Test Augmentation

Let’s say you have a magnificent test suite all set up, and your code passes all the tests. It’s glorious. But then, one day, one of the normally reliable tests inexplicably fails or crashes with a segmentation fault. There are two common reasons for these kinds of nondeterministic test failures: race conditions, where your test might fail only if two operations occur on different threads in a particular order, and undefined behavior in unsafe code, such as if some unsafe code reads a particular value out of uninitialized memory.

Catching these kinds of bugs with normal tests can be difficult—often you don’t have sufficient low-level control over thread scheduling, memory layout and content, or other random-ish system factors to write a reliable test. You could run each test many times in a loop, but even that may not catch the error if the bad case is sufficiently rare or unlikely. Luckily, there are tools that can help augment your tests to make catching these kinds of bugs much easier.

The first of these is the amazing tool Miri, an interpreter for Rust’s mid-level intermediate representation (MIR). MIR is an internal, simplified representation of Rust that helps the compiler find optimizations and check properties without having to consider all of the syntax sugar of Rust itself. Running your tests through Miri is as simple as running cargo miri test. Miri interprets your code rather than compiling and running it like a normal binary, which makes the tests run a decent amount slower. But in return, Miri can keep track of the entire program state as each line of your code executes. This allows Miri to detect and report if your program ever exhibits certain types of undefined behavior, such as uninitialized memory reads, uses of values after they’ve been dropped, or out-of-bounds pointer accesses. Rather than having these operations yield strange program behaviors that may only sometimes result in observable test failures (like crashes), Miri detects them when they happen and tells you immediately.

For example, consider the very unsound code in Listing 7-8, which creates two exclusive references to a value.

```
let mut x = 42;
let x: *mut i32 = &mut x;
let (x1, x2) = unsafe { (&mut *x, &mut *x) };
println!("{}", x1, x2);
```

Listing 7-8: Wildly unsafe code that Miri detects is incorrect

At the time of writing, if you run this code through Miri, you get an error that points out exactly what’s wrong:

```
error: Undefined Behavior: trying to reborrow for Unique at alloc1383, but parent tag <2772> does not have an appropriate item in the borrow stack
--> src/main.rs:4:6
|
4 | let (x1, x2) = unsafe { (&mut *x, &mut *x) };
| ^^ trying to reborrow for Unique at alloc1383, but parent tag <2772>
does not have an appropriate item in the borrow stack
```
