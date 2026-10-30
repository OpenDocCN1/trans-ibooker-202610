> if serde in this example has a generic method or type that you use in your crate, the code of that method or type will be monomorphized and optimized in your crate, and your crate’s profile settings will apply, not those in the profile override for serde.

# Conditional Compilation

Most Rust code you write is universal—it’ll work the same regardless of what CPU or operating system it runs on. But sometimes you’ll have to do something special to get the code to work on Windows, on ARM chips, or when compiling against a particular platform application binary interface (ABI). Or maybe you want to write an optimized version of a particular function when a given CPU instruction is available, or disable some slow but uninteresting setup code when running in a continuous integration (CI) environment. To cater to cases like these, Rust provides mechanisms for conditional compilation, in which a particular segment of code is compiled only if certain conditions are true of the compilation environment.

We denote conditional compilation with the cfg keyword that you saw in “Using Features in Your Crate” on page XX. It usually appears in the form of the #[cfg(condition)] attribute, which says to compile the next item only if condition is true. Rust also has #[cfg_attr(condition, attribute)], which is compiled as #[attribute] if condition holds and is a no-op otherwise. You can also evaluate a cfg condition as a Boolean expression using the cfg!(condition) macro.

Every cfg construct takes a single condition made up of options, like feature = "some-feature", and the combinators all, any, and not, which do what you would probably expect. Options are either simple names, like unix, or key/value pairs like those used by feature conditions.

There are a number of interesting options you can make compilation dependent on. Let’s go through them, from most common to least common:

# Feature options

You’ve already seen examples of these. Feature options take the form feature = "name-of-feature" and are considered true if the named feature is enabled. You can check for multiple features in a single condition using the combinators. For example, any(feature = "f1", feature = "f2") is true if either feature f1 or feature f2 is enabled.

# Operating system options

These use key/value syntax with the key target_os and values like windows, macos, and linux. You can also specify a family of operating systems using target_family, which takes the value windows or unix. These are common enough that they have received their own named short forms, so you can use cfg(windows) and cfg(unix) directly. For example, if you wanted a particular code segment to be compiled only on macOS and Windows, you would write: #[cfg(any(windows, target_os = "macos"))].