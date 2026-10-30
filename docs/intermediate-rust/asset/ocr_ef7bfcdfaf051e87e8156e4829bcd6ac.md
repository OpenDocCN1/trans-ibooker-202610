# Test-Only APIs

First, having test-only code allows you to expose additional methods, fields, and types to your (unit) tests so the tests can check not only that the public API behaves correctly, but also that the internal state is correct. For example, consider the HashMap type from hashbrown, the crate that implements the standard library HashMap. The HashMap type is really just a wrapper around a RawTable type, which is what implements most of the hash table logic. Suppose that after doing a HashMap::insert on an empty map, you want to check that a single bucket in the map is nonempty, as shown in Listing 7-2.

```
#[test]
fn insert_just_one() {
    let mut m = HashMap::new();
    m.insert(42, ());
    let full = m.table.buckets.iter().filter(Bucket::is_full).count();
    assert_eq!(full, 1);
}
```

Listing 7-2: A test that accesses inaccessible internal state and thus does not compile

This code will not compile as written, because while the test code can access the private table field of HashMap, it cannot access the also private buckets field of RawTable, as RawTable lives in a different module. We could fix this by making the buckets field visibility pub(crate), but we really don’t want HashMap to be able to touch buckets in general, as it could accidentally corrupt the internal state of the RawTable. Even making buckets available as read-only could be problematic, as new code in HashMap may then start depending on the internal state of RawTable, making future modifications more difficult.

The solution is to use #[cfg(test)]. We can add a method to RawTable that allows access to buckets only while testing, as shown in Listing 7-3, and thereby avoid adding footguns for the rest of the code. The code from Listing 7-2 can then be updated to call buckets() instead of accessing the private buckets field.

```
impl RawTable {
    #[cfg(test)]
    pub(crate) fn buckets(&self) -> &[Bucket] {
        &self.buckets
    }
}
```

Listing 7-3: Using #[cfg(test)] to make internal state accessible in the testing context

# Bookkeeping for Test Assertions

The second benefit of having code that exists only during testing is that you can augment the program to perform additional runtime bookkeeping that can then be inspected by tests. For example, imagine you’re writing your own version of the BufWriter type from the standard library. When testing it, you want to make sure that BufWriter does not issue system calls