
However, there are some cases where your processes start or open resources aren’t automatically stopped or closed when their “parent process” dies. A straightforward example could be a GenServer process that creates and opens a file when starting and uses it to dump some internal state during its life cycle. It’s likely that you would want this file to be deleted and cleaned up if the GenServer were to crash unexpectedly. This is something that we believe is worth testing, so we’ll explore it a bit later in this section.

When it comes to supervision trees, we believe the thing that *might* be worth testing is that your supervision tree is *laid out correctly*. However, we'll be frank here: that’s hard and messy to test. We'll explore some solutions and alternatives toward the end of this section, but we want to set the expectations pretty low.

## Testing Cleanup After Crashes

If you want to perform some cleanup after a process crashes in order to ensure that the crash doesn’t leave anything behind, your best option will often be a different and *very straightforward* process that monitors the first process and whose job is to perform only the necessary cleanup when the first process crashes. Let’s go back to the example we mentioned at the end of the last section: a GenServer that dumps some terms to a file during its life cycle to avoid keeping such state in memory:

```
testing_otp/cleanup_after_crashes/genserver_that_uses_file.ex

defmodule GenServerThatUsesFile do
  use GenServer

  def start_link(opts) do
    GenServer.start_link(__MODULE__, opts, name: __MODULE__)
  end

  def store(pid, term) do
    GenServer.cast(pid, {:store, term})
  end
```

```
end

@impl true
def init(opts) do
  path = Keyword.fetch!(opts, :path)

  File.touch!(path)

  pid = self()
  ref = make_ref()
  spawn_link(fn -> monitor_for_cleanup(pid, ref, path) end)

  # Wait for the cleanup process to be ready, so that if this process
  # crashes before the cleanup process is trapping exits then we don't
  # leave a zombie process.
  receive do
    {^ref, :ready} -> :ok
  end

  {:ok, path}
end

@impl true
def handle_cast({:store, term}, path) do
  new_content = "\n" <> :erlang.term_to_binary(term)
  File.write!(path, new_content, [:binary, :append])
  {:noreply, path}
end

defp monitor_for_cleanup(pid, ref, path) do
  Process.flag(:trap_exit, true)
  send(pid, {ref, :ready})

  receive do
    {:EXIT, ^pid, _reason} ->
      File.rm_rf!(path)
  end
end
end
```

This GenServer doesn’t do anything useful, but you can imagine how it could have an API to retrieve particular terms that it adds to the file, for

example. Let’s keep this possible API in our imagination for the sake of writing less code.

There’s no reliable way to make sure that if this GenServer crashes it’ll clean up the file. So, what we do is write a little “cleanup process.” This process could also crash, yes, but it’s less likely to do so given how simple its code is. We spawn this process directly from the GenServer’s init/1 callback. The code isn’t the most straightforward, but it’s just taking care of possible race conditions and ensuring the following:

- The GenServer process dies if—because of some freak accident—the cleanup process dies, and
- The cleanup process only removes the file when the GenServer process dies and then peacefully terminates.

Now that we have this process in place, testing the aftermath of it crashing is straightforward. We can just kill the GenServer process and make sure that the file isn’t there anymore:

testing_otp/cleanup_after_crashes/genserver_that_uses_file_test.exs

```
test "no file is left behind if the GenServer process crashes" do
  path =
    Path.join(
      System.tmp_dir!(),
      Integer.to_string(System.unique_integer([:positive]))
    )

  pid = start_supervised!({GenServerThatUsesFile, path: path})

  assert File.exists?(path)

  Process.exit(pid, :kill)

  wait_for_passing(2_seconds = 2000, fn ->
    refute File.exists?(path)
  end)
end
```

The test generates a random path in the system’s temporary directory (using System.tmp_dir!/0), then starts the GenServer and asserts that the file is there, kills the GenServer brutally (with Process.exit/2 and reason :kill), and finally asserts that the file isn’t there anymore. You’ll notice the use of a function called wait_for_passing/2. This is a little function we find ourselves writing pretty often when working on Elixir code. Its purpose is to avoid fancy message-passing in order to know when we can run an assertion.

wait_for_passing/2’s job is to run an assertion repeatedly for a maximum given interval of time (two seconds in this case), allowing it to fail during that interval. After the time interval, the assertion is run one last time without rescuing any exceptions so that if the assertion fails after the given time interval, then the test will fail. We need wait_for_passing/2 in this test because if we were to assert the non-existence of the file right after we killed the GenServer, we’d have a race condition that could result in the file not having been deleted yet when the assertion is run. By waiting for a couple of seconds and trying the assertion over and over, we’re giving what’s likely more than enough time to the cleanup process to delete the file. If after two seconds the file is still there, it means that we probably have a problem. Note that we could bump the interval to ten or twenty seconds or even more if we didn’t feel comfortable: wait_for_passing/2 returns as soon as the assertion passes, so our tests would remain fast unless the assertion would fail (which is unlikely to be our normal test run since we’d hopefully fix the bug and make it pass again!).

Let’s look at the code for this little helper function:

```
testing_otp/cleanup_after_crashes/genserver_that_uses_file_test.exs

defp wait_for_passing(timeout, fun) when timeout > 0 do
  fun.()
rescue
  _ ->
```

```
Process.sleep(100)
wait_for_passing(timeout - 100, fun)
end

defp wait_for_passing(_timeout, fun), do: fun.()
```

The implementation is straightforward and uses recursion. It decreases the given timeout until it “runs out.” In this implementation, we’re hard-coding Process.sleep(100) and timeout - 100, which means that the assertion is run every 100 milliseconds during the given interval, but we could change this value or turn it into an argument to the wait_for_passing function to make it customizable. wait_for_passing/2 isn’t the most efficient function since it repeatedly runs the assertion and “wastes” a few hundred milliseconds between runs, but it’s a good and effective tool that in the real world we’ve ended up using in more than a few places.

This section has turned out to be more about the code to test than the tests themselves, but we believe it serves the purpose of showing what it means to test cleaning up after crashes. In this instance, the test was small and simple enough. If things become more complicated, you’ll have the tools we learned about in the previous chapters to help you architect your tests using things like dependency doubles and message passing.

Let’s move on to the final enemy of easy testing in the OTP landscape: supervisors.

## Testing Supervision Trees

Supervisors are one of the strongest selling points of Erlang/Elixir and the OTP set of abstractions. They allow you to structure the life cycle of the processes inside your application in a resilient way, and they make isolating failures a breeze. However, supervisors are one of the toughest things to test that we’ve come across. The reason for this is that their main job is to allow your application to recover from complex

and cascading failures that are hard to trigger on purpose during testing.

Imagine having a complex and “deep” supervision tree (with several levels between the root of the tree and the leaf processes). Now imagine that a child in a corner of the tree starts crashing and doesn’t recover just by being restarted on its own. OTP works beautifully and propagates the failure to the parent supervisor of that child, which starts crashing and restarting all of its children. If that doesn’t solve the problem, then the failure is propagated up and up until restarting enough of your application fixes the problem (or the whole thing crashes, if it’s a really serious problem). Well, how do you test this behavior? Do you even want to extensively test it?

It’s hard to inject a failure during testing that isn’t solved by a simple restart but that also doesn’t bring down the whole application. At the same time, we know that supervisors work: that is, we know that the failure isolation and “bubble up” restarting behavior work. We know that because supervisors have been battle-tested for decades at this point. As already discussed, that’s not what we want to test because it falls in the antipattern of “testing the abstraction” instead of testing your own logic.

Our advice? In practice, we tend to just not have automated testing for supervision trees. We test the individual components of our applications, and sometimes those components are made of a set of processes organized in a “subtree” of the main application supervision tree. In order to stay true to the black box testing model, we often test the component as a whole regardless of whether it’s made of multiple processes living inside a supervision tree. If components are unit-tested and integration tested, then we happily rely on OTP to make sure that supervisors behave in the right way.

However, we don’t fly completely blind. Most of the time, we spend some time every now and then manually firing up our application and
