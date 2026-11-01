```
-     %{"email" => ^expected_email_address}
-     ]
-     }
-     ]
} = decoded_body
20:
-   end)
- end
```

As can be seen on line 2, the function is a wrapper around a pre-defined Mox call. The name is specific enough to tell us what it’s doing without us having to worry about the how. That can be useful, as a single, descriptive function call in our test can leave it more readable than dropping an entire Mox function setup inside a test. And, of course, it makes it reusable.

Notice that we don’t pin the arguments in the anonymous function that we pass to Mox.expect/3. Instead, we have assertions inside the anonymous function. That’s because when there’s more than one required value, pinning the value throws a FunctionClauseError on failure, which doesn’t highlight which value didn’t match. Having each value in a separate assertion will provide immediate, clear feedback as to which value is incorrect. That doesn’t mean you can’t pin values if you need to. You can see that on line 16. We’re still able to use a pinned value with a pattern match, but it’s happening inside the anonymous function and not in the function head.

Our test helper makes sure that the specifics to sending an email out to a customer are there, but it doesn’t go so far as to assert on the content of that email. It’s your decision whether you find additional assertions to be helpful there. The module to send email will need to be unit-tested on its own. We could make our helper function less generic, taking in a string as an argument and making sure that string was in the email body. We chose not to, though, as this test just needs to make sure that the email is sent, leaving the explicit testing to unit tests. Additionally,