any email your application sends should be manually tested to make sure that it renders correctly. As a result, getting more explicit in the endpoint test is redundant.

# Testing the Error Case for Create

We complete our coverage of this endpoint and controller action by testing a failure case. Inside the same describe block, and under our previous test, add this:

```
1: test "error: does not insert, redirects to 'new' page w/invalid attributes",
   %{
     conn: conn
   } do
5: flunk_if_email_is_sent()
   expected_user_count = Repo.all(User) |> Enum.count()
   conn = post(conn, Routes.user_path(conn, :create), user: %{})
   assert html = html_response(conn, 200)
10:
   parsed_html = Floki.parse_document!(html)
   for field <- ["name", "email", "password"] do
     # using <> so that the first #(indicating an id) is easy to read
     field_as_id = "#" <> "#{field}-error"
15:
     span = Floki.find(parsed_html, field_as_id)
     assert Floki.text(span) == "can't be blank"
   end
   assert Repo.all(User) |> Enum.count() == expected_user_count,
20:
     "There should have been no records inserted during this test."
   end
```

The test leverages a new custom helper function called flunk_if_email_is_sent/0, on line 5, that’s defined in NotSkull.ConnCase, right