
```toml
[dependencies]
foo = { version = "1", features = ["derive"] }
```

Listing 6-2: Enabling a feature of a dependency

Some features are used so frequently that it makes more sense to have a crate opt out of them rather than in to them. To support this, Cargo allows you to define a set of default features for a crate. And similarly, it allows you to opt out of the default features of a dependency. Listing 6-3 shows how foo can make its derive feature enabled by default, while also opting out of some of syn’s default features and instead enabling only the ones it needs for the derive feature.

```toml
[package]
name = "foo"
...
[features]
derive = ["syn"]
default = ["derive"]

[dependencies.syn]
version = "1"
default-features = false
features = ["derive", "parsing", "printing"]
optional = true
```

Listing 6-3: Adding and opting out of default features, and thus optional dependencies

Here, if a crate depends on foo and does not explicitly opt out of the default features, it will also compile foo’s syn dependency. In turn, syn will be built with only the three listed features, and no others. Opting out of default features this way, and opting in to only what you need, is a great way to cut down on your compile times!

## OPTIONAL DEPENDENCIES AS FEATURES

When you define a feature, the list that follows the equal sign is itself a list of features. This might, at first, sound a little odd—in Listing 6-3, syn is a dependency, not a feature. It turns out that Cargo makes every optional dependency a feature with the same name as the dependency. You’ll see this if you try to add a feature with the same name as an optional dependency; Cargo won’t allow it. Support for a different namespace for features and dependencies is in the works in Cargo, but has not been stabilized at the time of writing. In the meantime, if you want to have a feature named after a dependency, you can rename the dependency using package = "" to avoid the name collision. The list of features that a feature enables can also include features of dependencies. For example, you can write derive = ["syn/derive"] to have your derive feature enable the derive feature of the syn dependency.

unit; the Rust compiler treats a crate more or less as one big source file compiled as one chunk that is ultimately turned into a single binary output (either a binary or a library).

While this simplifies many aspects of the compiler, it also means that large crates can be painful to work with. If you change a unit test, a comment, or a type in one part of your application, the compiler must re-evaluate the entire crate to determine what, if anything, changed. Internally, the compiler implements a number of mechanisms to speed up this process, like incremental recompilation and parallel code generation, but ultimately the size of your crate is a big factor in how long your project takes to compile.

For this reason, as your project grows, you may want to split it into multiple crates that internally depend on one another. Cargo has just the feature you need to make this convenient: workspaces. A *workspace* is a collection of crates (often called *subcrates*) that are tied together by a top-level *Cargo.toml* file like the one shown in Listing 6-4.

```toml
[workspace]
members = [
    "foo",
    "bar/one",
    "bar/two",
]
```

*Listing 6-4: A workspace Cargo.toml*

The members array is a list of directories that each contain a crate in the workspace. Those crates all have their own *Cargo.toml* files in their own subdirectories, but they share a single *Cargo.lock* file and a single output directory. The crate names don’t need to match the entry in members. It is common, but not required, that crates in a workspace share a name prefix, usually chosen as the name of the “main” crate. For example, in the tokio crate, the members are called tokio, tokio-test, tokio-macros, and so on.

Perhaps the most important feature of workspaces is that you can interact with all of the workspace’s members by invoking cargo in the root of the workspace. Want to check that they all compile? cargo check will check them all. Want to run all your tests? cargo test will test them all. It’s not quite as convenient as having everything in one crate, so don’t go splitting everything into minuscule crates, but it’s a pretty good approximation.

> **NOTE**  
> *Cargo commands will generally do the “right thing” in a workspace. If you ever need to disambiguate, such as if two workspace crates both have a binary by the same name, use the -p flag (for package). If you are in the subdirectory for a particular workspace crate, you can pass --workspace to perform the command for the entire workspace instead.*

Once you have a workspace-level *Cargo.toml* with the array of workspace members, you can set your crates to depend on one another using path dependencies, as shown in Listing 6-5.

```toml
[patch.crates-io]
# use a local (presumably modified) source
regex = { path = "/home/jon/regex" }
# use a modification on a git branch
serde = { git = "https://github.com/serde-rs/serde.git", branch = "faster" }
# patch a git dependency
[patch.'https://github.com/jonhoo/project.git']
project = { path = "/home/jon/project" }
```

Listing 6-6: Overriding dependency sources in Cargo.toml using [patch]

Even if you patch a dependency, Cargo takes care to check the crate versions so that you don’t accidentally end up patching the wrong major version of a crate. If you for some reason transitively depend on multiple major versions of the same crate, you can patch each one by giving them distinct identifiers, as shown in Listing 6-7.

```toml
[patch.crates-io]
nom4 = { path = "/home/jon/nom4", package = "nom" }
nom5 = { path = "/home/jon/nom5", package = "nom" }
```

Listing 6-7: Overriding multiple versions of the same crate in Cargo.toml using [patch]

Cargo will look at the Cargo.toml inside each path, realize that /nom4 contains major version 4 and that /nom5 contains major version 5, and patch the two versions appropriately. The package keyword tells Cargo to look for a crate by the name nom in both cases instead of using the dependency identifiers (the part on the left) as it does by default. You can use package this way in your regular dependencies as well to rename a dependency!

Keep in mind that patches are not taken into account in the package that’s uploaded when you publish a crate. A crate that depends on your crate will use only its own [patch] section (which may be empty), not that of your crate!

## CRATES VS. PACKAGES

You may wonder what the difference between a package and a crate is. These two terms are often used interchangeably in informal contexts, but they also have specific definitions that vary depending on whether you’re talking about the Rust compiler, Cargo, crates.io, or something else. I personally think of a crate as a Rust module hierarchy starting at a root .rs file (one where you can use crate-level attributes like #![feature])—usually something like lib.rs or main.rs. In contrast, a package is a collection of crates and metadata, so essentially all that’s described by a Cargo.toml file. That may include a library crate, multiple binary crates, some integration test crates, and maybe even multiple workspace members that themselves have Cargo.toml files.

## [profile]

The [profile] section lets you pass additional options to the Rust compiler in order to change the way it compiles your crate. These options fall primarily into three categories: performance options, debugging options, and options that change code behavior in user-defined ways. They all have different defaults depending on whether you are compiling in debug mode or in release mode (other modes also exist).

The three primary performance options are opt-level, codegen-units, and lto. The opt-level option tweaks runtime performance by telling the compiler how aggressively to optimize your program (0 is “not at all,” 3 is “as much as you can”). The higher the setting, the more optimized your code will be, which *may* make it run faster. Extra optimization comes at the cost of higher compile times, though, which is why optimizations are generally enabled only for release builds.

> You can also set opt-level to "s" to optimize for binary size, which may be important on embedded platforms.

The codegen-units option is about compile-time *performance*. It tells the compiler how many independent compilation tasks (*code generation units*) it is allowed to split the compilation of a single crate into. The more pieces a large crate’s compilation is split into, the faster it will compile, since more threads can help compile the crate in parallel. Unfortunately, to achieve this speedup, the threads need to work more or less independently, which means code optimization suffers. Imagine, for example, that the segment of a crate compiling in one thread could benefit from inlining some code in a different segment—since the two segments are independent, that inlining can’t happen! This setting, then, is a trade-off between compile-time performance and runtime performance. By default, Rust uses an effectively unbounded number of codegen units in debug mode (basically, “compile as fast as you can”) and a smaller number (16 at the time of writing) in release mode.

The lto setting toggles *link-time optimization (LTO)*, which enables the compiler (or the linker, if you want to get technical about it) to jointly optimize bits of your program, known as *compilation units*, that were originally compiled separately. The exact details of LTO are beyond the scope of this book, but the basic idea is that the output from each compilation unit includes information about the code that went into that unit. After all the units have been compiled, the linker makes another pass over all of the units and uses that additional information to optimize the combined compiled code. This extra pass adds to the compile time but recovers most of the runtime performance that may have been lost due to splitting the compilation into smaller parts. In particular, LTO can offer significant performance boosts to performance-sensitive programs that might benefit from cross-crate optimization. Beware, though, that cross-crate LTO can add a lot to your compile time.

Rust performs LTO across all the codegen units within each crate by default in an attempt to make up for the lost optimizations caused by using
