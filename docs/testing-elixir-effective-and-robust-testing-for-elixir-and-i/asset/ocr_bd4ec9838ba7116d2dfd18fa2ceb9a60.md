```
1: describe "add_element/2" do
-   test "adding an element to a full list rolls a value" do
-   max_measurements = Enum.random(3..10)
-   {:ok, pid} = RollingAverage.start_link(max_measurements)
5:
-   # "Fill" the measurements with 4s.
-   for _ <- 1..max_measurements do
-     RollingAverage.add_element(pid, 4)
-   end
10:
-   assert %{size: ^max_measurements, measurements: measurements} =
-     :sys.get_state(pid)
-   
-   expected_measurements = List.duplicate(4, max_measurements)
    assert measurements == expected_measurements
15:
-   
-   # We add an element that we know causes an "overflow".
-   RollingAverage.add_element(pid, 1)
-   
-   assert %{size: ^max_measurements, measurements: measurements} =
20:
-     :sys.get_state(pid)
-   
-   expected_measurements = [1 | List.duplicate(4, max_measurements - 1)]
-   assert measurements == expected_measurements
    end
25:
-   end
```

First notice that the test is set up to work with a dynamic-size list. When given the option of static versus dynamic values in your tests, always tend toward dynamic ones. This choice makes it less likely that you’ll miss edge cases due to your test forcing your code to work only with specific values. In this case, we’re starting with a list of 4s, as long as the maximum number of measurements we pass to start_link/1 (this