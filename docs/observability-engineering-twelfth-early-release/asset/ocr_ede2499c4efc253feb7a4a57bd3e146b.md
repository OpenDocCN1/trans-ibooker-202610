Figure 14-3. Common dimensions in our Hack codebase. You can use these as examples for structuring dimensions across spans and services in CI.

For example, Slack engineers might want to identify concurrency issues along common dimensions (like hostnames or groups of Jenkins workers). The TraceContext already provides a hostname tag. The CI runner client then appends a tag for the Jenkins worker label. Using a combination of these two dimensions, Slack engineers can then group individual hosts or groups of Jenkins workers that have runtime issues.

Similarly, Slack engineers might want to identify common build failures. The CI runner client appends a tag for the commit head or commit main branch. This combination allows for identifying which commits a broken build might come from.

The dimensions in [Figure 14-3](#) are then used in various script and service calls as they communicate with one another to complete a test run ([Figure 14-4](#)).