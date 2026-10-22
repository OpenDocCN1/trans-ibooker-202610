# Anticorruption layer

Anticorruption layers can be useful for protecting bounded contexts from legacy systems, especially, when legacy systems are using inefficient models that tend to spread into downstream components.

Another common use case for implementing an anticorruption layer is to protect a bounded context from frequent changes in the public interfaces of an upstream service it uses.

# Open-host service

If changes in the implementation details of one component often ripple through the system and affect its consumers, consider making it an open-host service: decouple its implementation model from the public API it exposes.

# Separate ways

Especially in large organizations, you may encounter friction among engineering teams resulting from having to collaborate and co-evolve a shared functionality. If the “apple of discord” functionality is not business critical—that is, it’s not a core subdomain—the teams can go their separate ways and implement their own solutions, eliminating the source of friction.

# Tactical Modernization

First and foremost, from a tactical standpoint, look for the most “painful” mismatches in business value and implementation strategies, such as core subdomains implementing patterns that