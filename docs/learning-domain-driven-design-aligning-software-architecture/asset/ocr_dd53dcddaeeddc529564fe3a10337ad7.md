![](bbox=[0.14, 0.08, 0.86, 0.27])

Figure A-4. The event crunchers bounded context handling the incoming customer events

Since we didn’t make any money out of the way we move data around, and there weren’t any off-the-shelf solutions that could have been used, event crunchers resembled a supporting subdomain. We designed it as such.

Nothing fancy this time: just layered architecture and some simple transaction scripts. This solution worked great, but only for a while.

As our business evolved, we implemented more and more features in the event crunchers. It started with business intelligence (BI) people asking for some flags: a flag to mark a new contact, another one to mark various first-time events, some more flags to indicate some business invariants, and so on.

Eventually, those simple flags evolved into a real business logic, with complex rules and invariants. What started out as transaction scripts evolved into a full-fledged core business subdomain.

Unfortunately, nothing good happens when you implement complex business logic as transaction scripts. Since we didn’t adapt our design to cope with the complex business logic, we ended up with a very big ball of mud. Each modification to the codebase became more and more expensive, quality went downhill, and we were forced to rethink the event crunchers design. We did that a year later. By that time, the business logic had become so complex that it could only be tackled with event sourcing. We refactored the event