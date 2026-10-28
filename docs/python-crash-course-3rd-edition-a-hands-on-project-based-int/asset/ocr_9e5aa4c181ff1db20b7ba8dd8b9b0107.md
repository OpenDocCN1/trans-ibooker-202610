```
❶ > ll_project

  * Region (--region)
  The region where the project will be hosted
    --snip--
    [us-3.platform.sh] Moses Lake, United States (AZURE) [514 g
  C02eq/kWh]
❷ > us-3.platform.sh
  * Plan (--plan)
  Default: development
  Enter a number to choose:
    [0] development
    --snip--
❸ > 0

  * Environments (--environments)
  The number of environments
  Default: 3
❹ > 3

  * Storage (--storage)
  The amount of storage per environment, in GiB
  Default: 5
❺ > 5
```

The first prompt asks for a name for the project ❶, so we use the name `ll_project`. The next prompt asks which region we'd like the server to be in ❷. Choose the server closest to you; for me, that's `us-3.platform.sh`. For the rest of the prompts, you can accept the defaults: a server on the lowest development plan ❸, three environments for the project ❹, and 5GB of storage for the overall project ❺.

There are three more prompts to respond to:

```
Default branch (--default-branch)
The default Git branch name for the project (the production e
nvironment)
Default: main
❻ > main
```