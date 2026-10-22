
## Types of Subdomains

Just as a software system comprises various architectural components—databases, frontend applications, backend services, and others—subdomains bear different strategic/business values. Domain-driven design distinguishes between three types of subdomains: core, generic, and supporting. Let’s see how they differ from a company strategy point of view.

## Core subdomains

A core subdomain is what a company does differently from its competitors. This may involve inventing new products or services or reducing costs by optimizing existing processes.

Let’s take Uber as an example. Initially, the company provided a novel form of transportation: ridesharing. As its competitors caught up, Uber found ways to optimize and evolve its core business: for example, reducing costs by matching riders heading in the same direction.

Uber’s core subdomains affect its bottom line. This is how the company differentiates itself from its competitors. This is the company’s strategy for providing better service to its customers and/or maximizing its profitability. To maintain a competitive advantage, core subdomains involve inventions, smart optimizations, business know-how, or other intellectual property.

Consider another example: Google Search’s ranking algorithm. At the time of this writing, Google’s advertising platform accounts for the majority of its profits. That said, Google Ads is not a subdomain, but rather a separate business domain with subdomains comprising it, among its cloud computing service (Google Cloud Platform), productivity and collaboration tools (Google Workspaces), and other fields in which Alphabet, Google’s parent company, operates. But what about Google Search and its ranking algorithm? Although the search engine is not a paid service, it serves as the largest display platform for Google Ads. Its ability to provide excellent search results is what drives traffic, and subsequently, it is an important component of the Ads platform. Serving suboptimal search results due to a bug in the algorithm or a competitor coming up with an even better search service will hurt the ad business’s revenue. So, for Google, the ranking algorithm is a core subdomain.

## Complexity

A core subdomain that is simple to implement can only provide a short-lived competitive advantage. Therefore, core subdomains are naturally complex. Continuing with the Uber example, the company not only created a new marketspace with ridesharing, it disrupted a decades-old monolithic architecture, the taxi industry, through targeted use of technology. By understanding its business domain, Uber was able to design a more reliable and transparent method of transportation. There should be high entry barriers for a company’s core business; it should be hard for competitors to copy or imitate the company’s solution.

## Sources of competitive advantage

It’s important to note that core subdomains are not necessarily technical. Not all business problems are solved through algorithms or other technical solutions. A company’s competitive advantage can come from various sources.

Consider, for example, a jewelry maker selling its products online. The online shop is important, but it’s not a core subdomain. The jewelry design is. The company can use an existing off-the-shelf online shop engine, but it cannot outsource the design of its jewelry. The design is the reason customers buy the jewelry maker’s products and remember the brand.

As a more intricate example, imagine a company that specializes in manual fraud detection. The company trains its analysts to go over questionable documents and flag potential fraud cases. You are building the software system the analysts are working with. Is it a core subdomain? No. The core subdomain is the work the analysts are doing. The system you are building has nothing to do with fraud analysis, it just displays the documents and tracks the analysts’ comments.

## CORE SUBDOMAIN VERSUS CORE DOMAIN

Core subdomains are also called core domains. For example, in the original domain-driven design book, Eric Evans uses “core subdomain” and “core domain” interchangeably. Although the term “core domain” is used often, I prefer to use “core subdomain” for a number of reasons. First, it is a *subdomain*, and I prefer to avoid confusion with *business domains*. Second, as you will learn in Chapter 11, it’s not uncommon for subdomains to evolve over time and change their types. For example, a core subdomain can turn into a generic subdomain. Hence, saying that “a *generic* subdomain has evolved into a *core* subdomain” is more straightforward than saying “a generic *subdomain* has evolved into a core *domain*.”

## Generic subdomains

*Generic subdomains* are business activities that all companies are performing in the same way. Like core subdomains, generic subdomains are generally complex and hard to implement. However, generic subdomains do not provide any competitive edge for the company. There is no need for innovation or optimization here: battle-tested implementations are widely available, and all companies use them.

For example, most systems need to authenticate and authorize their users. Instead of inventing a proprietary authentication mechanism, it makes more sense to use an existing solution. Such a solution is likely to be more reliable and secure since it has already been tested by many other companies that have the same needs.

Going back to the example of a jewelry maker selling its products online, jewelry design is a core subdomain, but the online shop is a generic subdomain. Using the same online retail platform—the same generic solution—as its competitors would not impact the jewelry maker’s competitive advantage.

## Supporting subdomains

As the name suggests, *supporting subdomains* support the company’s business. However, contrary to core subdomains, supporting subdomains do not provide any competitive advantage.

For example, consider an online advertising company whose core subdomains include matching ads to visitors, optimizing the ads’ effectiveness, and minimizing the cost of ad space. However, to achieve success in these areas, the company needs to catalog its creative materials. The way the company stores and indexes its physical creative materials, such as banners and landing pages, does not impact its profits. There is nothing to invent or optimize in that area. On the other hand, the creative catalog is essential for implementing the company’s advertising management and serving systems. That makes the content cataloging solution one of the company’s supporting subdomains.

The distinctive characteristic of supporting subdomains is the complexity of the solution’s business logic. Supporting subdomains are simple. Their business logic resembles mostly data entry screens and ETL (extract, transform, load) operations; that is, the so-called CRUD (create, read, update, and delete) interfaces. These activity areas do not provide any competitive advantage for the company, and therefore do not require high entry barriers.

## Comparing Subdomains

Now that we have a greater understanding of the three types of business subdomains, let’s explore their differences from additional angles and see how they affect strategic software design decisions.

## Competitive advantage

Only core subdomains provide a competitive advantage to a company. Core subdomains are the company’s strategy for differentiating itself from its competitors.

Generic subdomains, by definition, cannot be a source for any competitive advantage. These are generic solutions—the same solutions used by the company and its competitors.

Supporting subdomains have low entry barriers and cannot provide a competitive advantage either. Usually, a company wouldn’t mind its competitors copying its supporting subdomains—this won’t affect its competitiveness in the industry. On the contrary, strategically the company would prefer its supporting subdomains to be generic, ready-made solutions, thus eliminating the need to design and build their implementation. You will learn in detail about such cases of supporting subdomains turning into generic subdomains, as well as other possible permutations, in Chapter 11. A real-life case study of such a scenario will be outlined in Appendix A.

The more complex the problems a company is able to tackle, the more business value it can provide. The complex problems are not limited to delivering services to consumers. A complex problem can be, for example, making the business more optimized and efficient. For example, providing the same level of service as competitors do, but at lower operational costs, is a competitive advantage as well.

## Complexity

From a more technical perspective, it’s important to identify the organization’s subdomains, because the different types of subdomains have different levels of complexity. When designing software, we have to choose tools and techniques that
