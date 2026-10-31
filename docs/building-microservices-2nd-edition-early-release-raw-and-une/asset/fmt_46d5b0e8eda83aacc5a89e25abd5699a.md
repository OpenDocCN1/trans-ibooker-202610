
# Consumer-First Microservice Architecture

Remember, one of the secrets to an effective microservice architecture is to embrace a consumer-first approach. Your microservices exist to be called by other consumers. Their needs are paramount, and if you are making changes to a microservice that are going to cause upstream consumers problems, this needs to be taken into account.

In some situations of course it might not be possible to change the consumers. I’ve heard from Netflix that they had issues (at least historically), with old set-top boxes using older versions of the Netflix APIs. These set-top boxes cannot be upgraded easily, so the old endpoints have to remain available unless and until the number of older set-top boxes drops to a level where they can have their support disabled. Decisions to stop old consumers being able to access your endpoints can sometimes end up being financial - how much money does it cost you to support the old interface, balanced against how much money you make from those consumers.

## Tracking Usage

Even if you do agree on a time by which consumers should stop using the old interface, would you know if they had actually stopped using it? Making sure you have logging in place for each endpoint your microservice exposes can help, as can ensuring that you have some sort of client identifier so you can chat to the team in question if you need to work with them to get them to migrate away from your old interface. This could be something as simple as asking consumers to put their identifer in the user agent header when making HTTP requests, or you could require that all calls go via some sort of API Gateway where clients need keys to identify themselves.

## Extreme Measures

So, assuming you know a consumer is still using an old interface that you want to remove, and they are dragging their heels about moving to the new version, what can you do about it? Well, the first thing to do is talk to them. Perhaps you can lend them a hand to make the changes happen. If all else fails, and they still don’t upgrade even after agreeing to, then there are some extreme techniques I’ve seen used.

In one large tech company, I discussed with them how they handled this issue. Internally, they had a very generous period of one year before old interfaces would be retired. I asked how they knew if consumers were still using the old interfaces, and they replied that they didn’t bother tracking that information really. After one year they just turned the old interface off. It was recognized internally that if this caused a consumer to break, then in that company it was accepted that it was the fault of the consuming microservice’s team - they’d had a year to make the change, and hadn’t done it. Of course, this approach won’t work for many (I said it was extreme!). It also leads to a large degree of inefficiency. By not knowing if the old interface was used, they denied themselves the opportunity to remove it before the year had passed. Personally, even if I was to suggest just turning the endpoint off after a certain period of time, I’d still definitely want tracking of who was going to be impacted.

Another extreme measure I saw was actually in the context of deprecating libraries, but it could also theoretically be used for microservice endpoints. The example given was of an old library that people were trying to retire from use inside the organization, in favour of a newer, better one. Despite lots of work, other teams were still dragging their heels. The solution was to insert a sleep in the old library, so that it responded more slowly to calls (with logging to show what was happening). Over time, the team just kept increasing the duration of the sleep, until eventually the other teams got the message. You obviously have to be extremely sure that you’ve exhausted other reasonable efforts to get consumers to upgrade before considering something like this!

# DRY and the Perils of Code Reuse in a Microservice World

One of the acronyms we developers hear a lot is DRY: don’t repeat yourself. Though its definition is sometimes simplified as trying to avoid duplicating code, DRY more accurately means that we want to avoid duplicating our system *behavior and knowledge*. This is very sensible advice in general. Having lots of lines of code that do the same thing makes your codebase larger than needed, and therefore harder to reason about. When you want to change behavior, and that behavior is duplicated in many parts of your system, it is easy to forget everywhere you need to make a change, which can lead to bugs. So using DRY as a mantra, in general, makes sense.

DRY is what leads us to create code that can be reused. We pull duplicated code into abstractions that we can then call from multiple places. Perhaps we go as far as making a shared library that we can use everywhere! It turns out though that sharing code in a microservice environment is a bit more involved than that. As always, we have more than one option to consider.

## Sharing Code Via Libraries

One of the things we want to avoid at all costs is overly coupling a microservice and consumers such that any small change to the microservice itself can cause unnecessary changes to the consumer. Sometimes, however, the use of shared code can create this very coupling. For example, at one client we had a library of common domain objects that represented the core entities in use in our system. This library was used by all the services we had. But when a change was made to one of them, all services had to be updated. Our system communicated via message queues, which also had to be drained of their now *invalid* contents, and woe betide you if you forgot.

If your use of shared code ever leaks outside your service boundary, you have introduced a potential form of coupling. Using common code like logging libraries is fine, as they are internal concepts that are invisible to the outside world. RealEstate.com.au makes use of a tailored service template to help bootstrap new service creation. Rather than make this code shared, the company copies it for every new service to ensure that coupling doesn’t leak in.

The really important point about sharing code via libraries is that you cannot update all uses of the library at once. Although multiple microservices might all use the same library, they do so typically by packaging that library into the microservice deployment. To upgrade the version of the library being used, you’d therefore need to redeploy the microservice. If you want to update the same library everywhere at exactly the same time, this could lead to a widespread deployment of multiple different microservices all at the same time, with all the associated headaches.

So, if using libraries for code reuse across microservice boundaries, you have to accept that multiple different versions of the same library might be out there at the same time. You can of course look to update all of these to the last version over time, but as long as you are OK with this fact, then by all means reuse code via libraries. If you really do need to update that code for all users of it at exactly the same time, then you’ll actually want to look at reusing code via a dedicated microservice instead.

There is one specific use case associated with reuse through libraries which is worth exploring further, though.

### Client Libraries

I’ve spoken to more than one team that has insisted that creating client libraries for your services is an essential part of creating services in the first place. The argument is that this makes it easy to use your service, and avoids the duplication of code required to consume the service itself.
