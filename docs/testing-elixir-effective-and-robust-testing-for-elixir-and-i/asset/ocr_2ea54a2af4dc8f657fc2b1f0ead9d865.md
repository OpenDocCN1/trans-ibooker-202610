```
end)

on_exit(fn ->
  IO.puts(
    "on_exit callback #2 (test #1) in process: #{inspect(self())}"
  )
end)

IO.puts("test #1 in process: #{inspect(self())}")
end

test "#2" do
  IO.puts("test #2 in process: #{inspect(self())}")
end
end

result = ExUnit.run()

IO.puts("The return value of ExUnit.run/0 is:")
IO.inspect(result)
```

The output is below.

```
setup_all #1 in process: #PID<0.109.0>
setup_all callback #2 in process: #PID<0.109.0>
setup callback #1 in process: #PID<0.110.0>
setup callback #2 in process: #PID<0.110.0>
test #2 in process: #PID<0.110.0>
setup callback #1 in process: #PID<0.111.0>
setup callback #2 in process: #PID<0.111.0>
test #1 in process: #PID<0.111.0>
on_exit callback #2 (test #1) in process: #PID<0.112.0>
on_exit callback #1 (test #1) in process: #PID<0.112.0>
The return value of ExUnit.run/0 is:
%{excluded: 0, failures: 0, skipped: 0, total: 2}
```

Maybe the visual representation below also helps: