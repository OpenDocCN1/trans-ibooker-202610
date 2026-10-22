first version of our software system had to implement the first one-third of our value chain:

+   - A system for managing contracts and integrations with external publishers
+   - A catalog for our designers to manage creative materials
+   - A campaign management solution to run advertising campaigns

I was overwhelmed and had to find a way to wrap my head around all the complexities of the business domain. Fortunately, not long before we started working, I read a book that promised just that. Of course, I’m talking about Eric Evans’s seminal work, *Domain-Driven Design: Tackling Complexity at the Heart of Software.*

If you have read this book’s Preface, you know Evans’s book provided the answers I’d been seeking for quite a while: how to design and implement business logic. That said, for me it wasn’t an easy book to comprehend on the first read. Nevertheless, I felt like I’d already gotten a strong grasp of DDD just by reading the tactical design chapters.

Guess how the system was initially designed? It would definitely make a certain prominent individual¹ from the DDD community very proud.

# Bounded Context #1: Marketing

The architectural style of our first solution could be neatly summarized as “aggregates everywhere.” Agency, campaign, placement, funnel, publisher: each and every noun in the requirements was proclaimed as an aggregate.

All of those so-called aggregates resided in a huge, lone, bounded context. Yes, a big, scary monolith, the kind everyone warns you about nowadays.