# Wrapping Up

In this chapter, we covered testing controllers and Phoenix Channels as the interfaces to our Phoenix-based web application. Controller tests are used in applications that serve JSON-based API endpoints as well as server-rendered HTML endpoints. Channel tests cover the interactions over web sockets. You should now be familiar with using the most common test-helper functions provided by Phoenix, Plug, and ChannelTest. You should also understand the how and why of making sure your tests cover both return values and side effects.

In the next chapter, we’ll introduce you to property-based testing. We’ll show you tooling that will help you lock down the behavior of your code where it’s most likely to have large variation in its input.

# Footnotes

+   - [37] https://github.com/ninenines/ranch
+   - [38] https://github.com/ninenines/cowboy
+   - [39] https://github.com/elixir-plug/plug
+   - [40] https://github.com/phoenixframework/phoenix
+   - [41] https://github.com/elixir-ecto/ecto
+   - [42] https://github.com/elixir-plug/plug
+   - [43] https://jwt.io/
+   - [44] https://hex.pm/packages/floki
+   - [45] https://www.npmjs.com/package/phoenix
+   - [46] https://github.com/phoenixframework/phoenix/blob/c8883af5582a38496e4b7e45e05d3a4d759a6caa/lib/phoenix/test/channel_test.ex#L269