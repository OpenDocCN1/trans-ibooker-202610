bind/2 is powerful because it lets us create complex generators by combining simpler generators. We’ll see that it’s heavily used in the context of property-based testing. bind/2 is also necessary in cases such as this one: we wouldn’t otherwise be able to take two generators and combine them together.

Next, let’s look at how generators can control the complexity of generated data through generation size.

# Generation Size

If you take a few elements out of a generator like StreamData.integer/0, you’ll notice that those integers are simple, that is, they are small integers centered around zero. Now, try to take a few elements out of integer/0 but after discarding a lot of elements:

```
iex> StreamData.integer() |> Stream.drop(100) |> Enum.take(5)
[43, -96, 45, -17, 40]
```

As you can see, the integers produced by the generator are now more complex—as in, they’re bigger. This suggests that generators produce increasingly more complex terms the more terms they generate. In the case of streaming terms out of a generator, that’s exactly what happens. However, the mechanism behind this is more generic. In order to have a generator generate a term, we need to pass a size to the generator that we refer to as the generation size. The generation size is a positive integer. Generators use the generation size as a measure of the complexity of the generated terms. Some generators will need to be tweaked to your use case so that they generate the “right” data, so to use most property-based testing frameworks effectively (including stream_data), it’s worth spending just a moment understanding generation size better.

Let’s look at how integer/0 uses generation size. With a generation size of n, integer/0 will produce an integer between -n and n. Other generators