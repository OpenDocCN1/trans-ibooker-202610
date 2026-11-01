common CI platforms, such as CircleCI[59] (mix coveralls.circle) or Semaphore[60] (mix coveralls.semaphore). Additionally, Excoveralls allows you to post to any Coveralls-compatible host via mix coveralls.post. For example, OpenCov[61] is a solid self-hosted coverage platform compatible with the Coveralls API.

We briefly looked at what test coverage is, why it’s useful, and how it can be used for Elixir projects. Test coverage is just one possible tool to increase confidence in your test suite, but it’s not necessarily a guarantee that your test suite is solid and well written. However, it can be useful to spot areas of your code that aren’t well tested or, at the other end of the spectrum, areas of your code that are “hot paths” and are executed many times during testing. In our experience, a sky-high test coverage (somewhere around 95% and above) doesn’t necessarily translate to a healthy and functioning codebase. Once the code coverage is high enough, striving to increase it might start to have diminishing returns. On the other hand, a low coverage (for example, somewhere around 40% or 50%) can point to an undertested codebase that might need some work. In general, the usefulness of test coverage varies from project to project, but it’s a useful tool to have in our testing tool belt.

# Footnotes

+   - [56] https://coveralls.io
+   - [57] https://coveralls.io
+   - [58] https://github.com/parroty/excoveralls
+   - [59] https://circleci.com
+   - [60] https://semaphoreci.com
+   - [61] https://github.com/danhper/opencov