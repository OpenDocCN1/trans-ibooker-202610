
# Artificial intelligence for core business data

## This chapter covers

- Looking at business data in terms of dollar density and business impact
- Using supervised learning to predict key information
- Comparing machine learning to conventional software engineering
- Case studies: Using AI to create new business lines and optimize processes

This chapter covers *core business data*, the type of data that’s closest to the value proposition of your business. Core business data is the secret sauce that makes your organization tick. It’s the order data collected by commercial operations, the human interactions tracked by social networks, and the blood-pressure measurements tracked by a health-care startup. Because core business data is at the heart of the organization, it’s also a prime candidate for your first AI applications. Doing AI on core business data is like cheating: the data is so close to the value-generating engine of the organization that successful projects are almost guaranteed to make a dramatic impact.

To whet your appetite, let’s briefly mention the two case studies you’ll find at the end of this chapter. The first one is about an initiative of Jim Gao, an ex-Google employee in charge of operations at its data centers. He looked at the data collected from the large air-conditioning systems used to cool Google’s gigantic computers and thought about using machine learning to optimize their consumption. The result was a 40% bill cut for the tech giant. The second case is about Square, a payment services company based in San Francisco. Square had been processing credit card payments for small businesses and realized that all the data it was collecting could be used to offer customized and low-risk loans to small businesses. By using machine learning models, Square created an entirely new business line and tackled the blue ocean of small business loans. The quality of its service is unmatched: by automating the lender-vetting process, Square can deposit loans to a customer’s bank account just one business day after the request is submitted. As a result, Square loaned more than $3 billion in four years, with exceptionally low delinquency rates of 4%. By the end of this chapter, you’ll know how these companies made these changes possible.

## 2.1 Unleashing AI on core business data

We define core business data as “data with a direct impact on the top or bottom line of the organization.” Core data looks very different depending on what your organization does: cart history for an e-commerce operation, physical measurements for an engineering organization, and patient behavior for a health-care company. Regardless of its form, core data is valuable because it describes events and patterns that have a direct impact on the organization’s performance, and it’s easy to attach a monetary value to it.

We already mentioned the two case studies that await you at the end of this chapter. Let’s look at why the data used by the two companies is core business data:

- A company like Google relies on massive data centers to offer its services (process web searches, store photos, route emails, and so forth). Probably the only variable cost for Google is the energy spent to keep the data centers’ computers cool. Therefore, data about its cooling plants is core to Google’s business, as it correlates directly to one of its main costs.
- Square’s core product is its point-of-sale (POS) solution. Through that product, Square processes all the payments going to its customers. Because Square’s mission is empowering small businesses, its transaction data is strictly linked to that vision and is valuable for its customers.

A good way to look at the value of data is to think about a metric we’ll call the dollar density of data: how much the data influences the top or bottom line of the organization. Core business data has a high dollar density: each e-commerce order, job lead, or financial transaction has a direct impact on your top or bottom line. As you move away from the core value proposition of your organization, the dollar density of the data you collect decreases accordingly. Recording visits to your website can be valuable, but not nearly as much as tracking orders coming in. For a hospital, the call center data could be useful, but not nearly as much as patient records. This is why we have decided to start the book with the highest-dollar-density data of all: your core business data.

Often we see that the core business data takes a structured form, just like the tidy rows and columns of a Microsoft Excel spreadsheet. In engineering terms, we call this type of data structured. Other examples of structured data are weather reports, measurements of physical processes, financial transactions, most markets, and supply chain and warehousing metrics. As a rule of thumb, anything that you can load in Excel is likely to fall within the structured data umbrella. Other types of data are harder to fit into neat Excel columns: think about pictures, voice recordings, or text in a book.

It’s important to understand that the same information can exist in both structured and unstructured forms. For example, consider the following way of recording a medical diagnosis:

> The patient Gianluca Mauro has a severe inflammation of the shoulder joint; the therapy is to take two pills of Cortisone per day for five days.

The same information can be recorded in a structured way, as in table 2.1.

### Table 2.1 A medical diagnosis in structured form

| Patient | Diagnosis | Area | Medication | Frequency (times/day) | Therapy length (days) |
|---|---|---|---|---|---|
| Gianluca Mauro | Severe inflammation | Shoulder joint | Cortisone | 2 | 5 |

The information is the same—but in the first case, it’s represented in an unstructured way (text), whereas in the second, it’s structured. Structured data is much easier for computers to process than unstructured data. Later chapters cover AI techniques for unstructured data, including images and written language. For now, let’s stick to structured data, happily knowing that most core business data falls into this category.

## 2.2 Using AI with core business data

Now that you know how to find and recognize core business data, let’s look at what AI can do with it. Because we believe in the teaching power of stories, we developed a simplified example that’s going to keep you company throughout this first part of the book.

FutureHouse is a fictional business that operates an online real estate marketplace where homeowners can advertise their homes for sale and hopefully attract interested buyers. FutureHouse has always prioritized customer service and employs agents who can offer their support to help sellers assess their house price. Buyers use the site to look for the house of their dreams.

We chose this example because it will be familiar to many readers, while giving us the opportunity to explore many aspects of the AI landscape. A nice bonus is that the housing market is a typical example in the ML literature. Should you decide to delve deeper into the technicalities of ML and maybe write your own code, you'll find plenty of references on the web.

We’ll start by explaining a bit about how housing markets work, and then we’ll introduce an application of ML to this business. We’re going to use this description as an excuse to introduce key terminology and concepts, and conclude by extending these concepts to other, more general applications.

### 2.2.1 The real estate marketplace example

Real estate agents are at the center of housing markets. They connect sellers and buyers and help them figure out the right price for their property. Figure 2.1 shows how the typical home-sale transaction unfolds under the careful watch of a realtor:

1. A customer comes in looking to sell their house. Before they put it on the market, they want a professional opinion about how much it’s worth.
2. The realtor checks out the house and sets a price based on its square footage, age, included appliances, other offers in the neighborhood, and so on.
3. The house gets listed.
4. Potential buyers find the listing and come in for an open house event. One lucky buyer eventually gets to buy the property.

![](img/51e9508eebbc565953ed15bd4b5bec8f_37_0.png)

FutureHouse gets to collect data at each step of this process. These are its main data assets:

- Pictures of all homes for sale
- Descriptions of the homes (square footage, location, and so forth)
- Web visit and click history for all users of the website
