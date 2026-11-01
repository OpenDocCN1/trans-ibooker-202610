```
iex> kv_store = KVStore.new()
iex> KVStore.set(kv_store, "key1", "some value")
iex> KVStore.get(kv_store, "key1")
"some value"
iex> KVStore.delete(kv_store, "key1")
iex> KVStore.get(kv_store, "key1")
nil
```

This is a stateful system where the state is the set of key-value pairs stored in the key-value store created with KVStore.new/0. One thing we could test about this system is that retrieving an existing key always returns the last value set for that key or a null value if the last operation was to delete the value, no matter how many times we set or delete that key. How can we model our key-value store in order to test this property? We can start with a simple two-element tuple {key, value} that stores the key we’re interested in and its current value (or nil if the key hasn’t been set yet). Let’s use pseudocode to see how we could generate a command assuming we’ve already generated a random key called random_key:

```
property_based_testing/misc/stateful_testing.exs

def command do
  one_of([
    command(:set, [random_key, term()]),
    command(:delete, [random_key])
  ])
end
```

For every possible command that we can issue on the stateful system, we need to define what happens to our model. Let’s continue with pseudocode:

```
property_based_testing/misc/stateful_testing.exs

def set({key, _old_value}, key, new_value) do
  {key, new_value}
end
```