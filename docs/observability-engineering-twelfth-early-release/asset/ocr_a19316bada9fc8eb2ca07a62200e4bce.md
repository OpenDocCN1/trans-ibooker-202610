to keep open, and keep those options open. Therefore, most of the time, you should choose boring technology. Don’t confuse boring with bad. Boring technology simply means that its edge cases and failure scenarios are well understood by many people.

However, with the demands of today’s users, more of us are discovering that many of the problems we need to solve cannot be solved by the humble-yet-mighty LAMP stack. This could be the case for a multitude of reasons. You might have higher reliability needs requiring resiliency guarantees that a LAMP stack can’t provide. Maybe you have too much data or data that’s too complex to be well served by a LAMP stack. Usually, we reach the edges of that simple architectural model in service of scale, reliability, or speed—that’s what drives us to shard, partition, or replicate our stacks.

The way we manage our code also matters. In larger or more complex settings, the monorepo creates organizational pressures that can drive technical change. Splitting up a code base can create clearer areas of ownership in ways that allow an organization to move more swiftly and autonomously than it could if everyone was contributing to The One Big App.

These types of needs—and more—are part of what has driven modern shifts in systems architecture that are clear and pronounced. On a technical level, these shifts include several primary effects:

+   - The decomposition of everything, from one to many
+   - The need for a variety of data stores, from one database to many storage systems
+   - A migration away from large monolithic applications, toward many smaller microservices
+   - A variety of infrastructure types, away from “big iron” servers, toward containers, functions, serverless, and other ephemeral, elastic resources