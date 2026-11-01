
## Introducing stream_data

stream_data is a property-based testing framework for Elixir. It provides two main functionalities, data generation and a framework for writing and running properties. The data generation aspect of the library is usable outside of property-based testing as a standalone feature, but it’s the backbone of the whole framework and is also used extensively when writing properties.

To follow along in the next few sections, create a new Mix project with $ mix new sorting and then add :stream_data as a dependency in your mix.exs file:

```
property_based_testing/sorting/mix.exs

defp deps do
  [{:stream_data, ">= 0.0.0", only: [:dev, :test]}]
end
```

Now, run $ mix deps.get to fetch the dependency. As you can see in the code, we’ve only added :stream_data in the :test environment since we’ll only be using the library when testing.

Before diving into the framework, let’s rewrite the RandomizedSortTest test we hand-rolled earlier to use the tools that stream_data provides:

```
property_based_testing/sorting/test/randomized_sort_stream_data_test.exs

1: defmodule FirstStreamDataPropertySortTest do
   use ExUnit.Case
   use ExUnitProperties

5:   property "Enum.sort/1 sorts lists" do
     check all list <- list_of(integer()) do
       sorted_list = Enum.sort(list)
```

```
-   assert length(list) == length(sorted_list)
    assert sorted?(sorted_list)
10:
-   end
-   end
- 
-   defp sorted?([first, second | rest]),
    do: first <= second and sorted?([second | rest])
15:
- 
-   defp sorted?(_other), do: true
-   end
```

Don’t worry about the new things you see in this test. We’ll cover all of them in this chapter. The goal here is to show you what stream_data looks like. For now, run mix test in the project where you added this file and see the beautiful green dots.

As it turns out, the underlying shape of the test is quite similar to RandomizedSortTest. Instead of using the test macro to define a test, we use property (on line 5). Then we use a new construct, check all, on line 6. This replaces the for comprehension we had. On the same line, we have list <- list_of(integer()). That’s exactly one of the most important features of a property-based framework: data generators. Here stream_data takes care of generating random data (with cool characteristics we’ll see later) for you. Now that we have an idea of what a stream_data test looks like, let’s move on to dissecting its components in a more detailed way.

In the next sections, we’re going to start exploring from the data generation aspect of stream_data and then move on to designing and running properties. To follow along, run iex -S mix to fire up an IEx session from the root of the project that includes stream_data as a dependency.

You might be wondering why we won’t illustrate these concepts on one of the applications we developed in the previous chapters (such as Soggy Waffle). Well, the reason is that we would have to bend those applications in weird ways to be able to show these ideas effectively. Instead, we decided to use simple, small, and self-contained examples so that we can focus on property-based testing concepts and tools.

## Data Generation

One of the design goals of stream_data is to provide a set of tools for data generation that could also work outside of property-based testing. For example, generating random data can be useful when seeding databases with fake data.

At the core of data generation are generators. A generator is a data structure that contains logic that stream_data uses to generate data. Essentially, a generator is like a function that we can call to generate random terms. Let’s start with a simple instance of a generator, StreamData.integer/0. This generator produces random integers. We can use all the stream_data generators as Elixir streams since they implement the Enumerable protocol.

```
iex> StreamData.integer() |> Enum.take(5)
[-1, 2, -3, 0, 2]
```

stream_data generators are infinite streams of random data, so we only had to take a few items out of the stream using Enum.take/2 in this example. If we had called Enum.to_list/1 passing the generator as the argument, we would’ve waited forever.

stream_data comes equipped with a few generators for simple data types, like the ones for integers or booleans, plus a bunch of ways to combine generators. For example, there’s no built-in generator for non-negative integers (positive integers plus zero), but we can easily build one using StreamData.map/2 to map the abs/1 function over the StreamData.integer/0 generator. This is similar to how Enum.map/2 maps a function over an enumerable.

```
non_negative_integer = StreamData.map(StreamData.integer(), &abs/1)
```

Wait, why are we using StreamData.map/2 instead of Stream.map/2, given that generators are Elixir streams? Stream.map/2 would work in the

context of generating data for purposes that lie outside of property-based testing, such as seeding a database with random data:

```
non_negative_integer = Stream.map(StreamData.integer(), &abs/1)
```

In general, all functions that operate on streams (such as many functions found in the Enum and Stream standard-library modules) work on stream_data generators. However, you should only use those functions in the context of data generation and not in property-based tests. When using generators in property-based tests, using Stream functions instead of StreamData ones will render the generators unusable. In the example above, Stream.map/2 would return a plain stream of non-negative integers, while StreamData.map/2 would return a stream of non-negative integers that also provides specific functionality for property-based testing. We'll learn more about this a little later, when we talk about shrinking the generated data.

Mapping is just one of the ways to combine generators. There’s also filtering as well as a few ways to create more complex data types based off simpler generators. An example is StreamData.list_of/1, which takes a generator and returns a new generator that produces lists of elements produced by the generator we passed to it. For example, to generate lists of integers we can use the StreamData.list_of(StreamData.integer()) generator.

```
iex> StreamData.list_of(StreamData.integer()) |> Enum.take(3)
[[1], [], [1, -2]]
```

## The Basic Tool for Composing Generators: Binding

While there are a few ways to compose generators, like StreamData.map/2, it’s important to know about StreamData.bind/2. bind/2 is a powerful function, and other ways of combining generators can usually be built on top of it. It’s the basic tool you can reach for when you need to combine two generators together. bind/2 takes a generator and a

function: the function will be invoked with each element produced by the generator and should return a new generator.

Let’s make this easier with an example. Say we have a generator that produces random email domains (like gmail.com or icloud.com). We want to generate random emails that have a random alphanumeric username followed by a random domain produced by our domain generator:

```
property_based_testing/misc/random_email_generator.exs

```exs
1: domains = ["gmail.com", "yahoo.com", "icloud.com"]
2: random_domain_generator = StreamData.member_of(domains)
3: username_generator = StreamData.string(:alphanumeric, min_length: 1)
4:
5: random_email_generator =
6:   StreamData.bind(random_domain_generator, fn domain ->
7:     StreamData.map(username_generator, fn username ->
8:       "#{username}@#{domain}"
9:     end)
10:   end)
```
```

In the first couple of lines, we can see our random domain generator, which uses StreamData.member_of/1 to produce elements taken at random from the given list. On line 3, we have our username generator, which produces alphanumeric strings. Then we have our bind/2 call. We bind on the value produced by the random domain generator, which is passed to the function we give to bind/2. That function should return a new generator. On line 9, we start with the username generator and map a function that concatenates an @ plus the domain to the values that the username generator produces. The result of the StreamData.map/2 call is a generator. When random_email_generator needs to produce a random email, it will first create a random domain, then use the generator returned by the anonymous function passed to bind/2 to produce a random email.
