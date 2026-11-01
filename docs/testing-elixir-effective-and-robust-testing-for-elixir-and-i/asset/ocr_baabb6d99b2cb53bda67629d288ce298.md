Our tests will use a common setup block, as most of them need two things: a user to exist in the database and a signed JWT for that user. It’s good to default to having your setup in each test so that the test reads without having to look elsewhere. If you have multiple tests that need the same setup, though, it can be useful to move that common code into a single place, in this case the setup block. The JWT is included as a header in the Conn, as seen on line 10. The actual JWT is created by a function defined in NotSkullWeb.ConnCase (called on line 14), as we need to share some of its logic with the other controller test file, for the login controller. While diving into how the JWT is created isn’t terribly helpful, adding things to request headers with put_req_header (line 11) is a very common thing to do in tests like these. That function comes from Plug.Conn and is imported via NotSkullWeb.ConnCase.

Unlike most of the tests we’ve written in previous chapters, our setup block accepts a context. This context comes from the test case. The generated file has a function that looks like this:

```
testing_phoenix/not_skull/test/support/conn_case.ex

1: setup tags do
  -   Mox.verify_on_exit!()
  - 
  -   :ok = Ecto.Adapters.SQL.Sandbox.checkout(NotSkull.Repo)
5:
  -   unless tags[:async] do
  -     Ecto.Adapters.SQL.Sandbox.mode(NotSkull.Repo, {:shared, self()})
  -   end
  - 
      {:ok, conn: Phoenix.ConnTest.build_conn()}
10:
  -   end
```

The first behavior worth noting is that it sets the sandbox mode to :shared (line 7). This setting is important because it impacts how your tests can be run. Shared mode allows any process to access the same