We understand how “unclean” this practice can feel. Luckily, during our professional careers we haven’t encountered too many cases where we had to resort to such extreme measures. In those few cases, though, we found this approach to be the “least terrible” solution, and we like it much more than the different-supervision-trees solution. Least terrible is far from best, but it’s better than most terrible, right?

The truth is, the more intertwined and complex the code gets and the more it interacts with complex parts of the world (like the host machine, or *time itself* when talking about periodic actions), the harder it gets to have control over those parts of the world and make testing clean. The good news is that there are some workarounds to these problems. The bad news is that they aren’t always nice and tidy.

Now that we’ve covered how to test GenServers, periodic actions, and singleton resources, let’s move on to testing another not-test-friendly Elixir and Erlang feature, *resiliency*.

That is, let’s talk about testing what happens when things go bad at the process level.