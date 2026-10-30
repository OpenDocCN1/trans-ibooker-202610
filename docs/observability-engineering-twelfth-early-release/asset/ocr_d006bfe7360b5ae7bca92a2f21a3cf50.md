# Chapter 15. Build Versus Buy and Return on Investment

# A NOTE FOR EARLY RELEASE READERS

With Early Release ebooks, you get books in their earliest form—the authors’ raw and unedited content as they write—so you can take advantage of these technologies long before the official release of these titles.

This will be the 15th chapter of the final book.

If you have comments about how we might improve the content and/or examples in this book, or if you notice missing material within this chapter, please reach out to the editor at [vwilson@oreilly.com](mailto:vwilson@oreilly.com).

So far in this book, we’ve examined both the technical fundamentals of observability and the social steps necessary to initiate the practice. In this part of the book, we will examine the considerations necessary when implementing observability at scale. We’ll focus on the functional requirements that are necessary to achieve the observability workflows described in earlier parts.

At a large enough scale, the question many teams will grapple with is whether they should build or buy an observability solution. Observability can seem relatively inexpensive on the surface, especially for smaller deployments. As user traffic grows, so too does the infrastructure footprint and volume of events your application generates. When dealing with substantially more observability data and seeing a much larger bill from a vendor, teams will start to consider whether they can save more by simply building an observability solution themselves.