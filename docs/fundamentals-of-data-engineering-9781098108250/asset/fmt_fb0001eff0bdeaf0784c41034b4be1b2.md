
# Data Ingestion Formats and Methods

## Robust Export Formats

More robust and expressive export formats include `Parquet`, `Avro`, `Arrow`, `ORC`, and `JSON`. These formats natively encode schema information and handle arbitrary string data with no particular intervention. Many of them also handle nested data structures natively so that `JSON` fields are stored using internal nested structures rather than simple strings. For columnar databases, columnar formats (`Parquet`, `Arrow`, `ORC`) allow more efficient data export because columns can be directly transcoded between formats. These formats are also generally more optimized for query engines. The `Arrow` file format is designed to map data directly into processing engine memory, providing high performance in data lake environments.

The disadvantage of these newer formats is that many of them are not natively supported by source systems. Data engineers are often forced to work with `CSV` data and then build robust exception handling and error detection to ensure data quality on ingestion. See [Appendix A](#app01.html#a_serialization_and_compression_technic) for a more extensive discussion of file formats.

## Shell

The `shell` is an interface by which you may execute commands to ingest data. The shell can be used to script workflows for virtually any software tool, and shell scripting is still used extensively in ingestion processes. A shell script might read data from a database, reserialize it into a different file format, upload it to object storage, and trigger an ingestion process in a target database. While storing data on a single instance or server is not highly scalable, many of our data sources are not particularly large, and such approaches work just fine.

In addition, cloud vendors generally provide robust CLI-based tools. It is possible to run complex ingestion processes simply by issuing commands to the `AWS CLI`. As ingestion processes grow more complicated and the SLA grows more stringent, engineers should consider moving to a proper orchestration system.

## SSH

`SSH` is not an ingestion strategy but a protocol used with other ingestion strategies. We use SSH in a few ways. First, SSH can be used for file transfer with `SCP`, as mentioned earlier. Second, SSH tunnels are used to allow secure, isolated connections to databases.

Application databases should never be directly exposed on the internet. Instead, engineers can set up a bastion host—i.e., an intermediate host instance that can connect to the database in question. This host machine is exposed on the internet, although locked down for minimal access from only specified IP addresses to specified ports. To connect to the database, a remote machine first opens an SSH tunnel connection to the bastion host and then connects from the host machine to the database.

## SFTP and SCP

Accessing and sending data both from secure FTP (`SFTP`) and secure copy (`SCP`) are techniques you should be familiar with, even if data engineers do not typically use these regularly (IT or security/secOps will handle this).

Engineers rightfully cringe at the mention of `SFTP` (occasionally, we even hear instances of `FTP` being used in production). Regardless, `SFTP` is still a practical reality for many businesses. They work with partner businesses that consume or provide data using `SFTP` and are unwilling to rely on other standards. To avoid data leaks, security analysis is critical in these situations.

`SCP` is a file-exchange protocol that runs over an SSH connection. `SCP` can be a secure file-transfer option if it is configured correctly. Again, adding additional network access control (defense in depth) to enhance `SCP` security is highly recommended.

## Webhooks

`Webhooks`, as we discussed in [Chapter 5](#ch05.html#data_generation_in_source_systems), are often referred to as *reverse APIs*. For a typical REST data API, the data provider gives engineers API specifications that they use to write their data ingestion code. The code makes requests and receives data in responses.

With a webhook ([Figure 7-15](#ch07.html#a_basic_webhook_ingestion_architecture)), the data provider defines an API request specification, but the data provider *makes API calls* rather than receiving them; it's the data consumer's responsibility to provide an API endpoint for the provider to call. The consumer is responsible for ingesting each request and handling data aggregation, storage, and processing.

![](assets/fode_0715.png)

Webhook-based data ingestion architectures can be brittle, difficult to maintain, and inefficient. Using appropriate off-the-shelf tools, data engineers can build more robust webhook architectures with lower maintenance and infrastructure costs. For example, a webhook pattern in AWS might use a serverless function framework (`Lambda`) to receive incoming events, a managed event-streaming platform to store and buffer messages (`Kinesis`), a stream-processing framework to handle real-time analytics (`Flink`), and an object store for long-term storage (`S3`).

You'll notice that this architecture does much more than simply ingest the data. This underscores ingestion's entanglement with the other stages of the data engineering lifecycle; it is often impossible to define your ingestion architecture without making decisions about storage and processing.

## Web Interface

Web interfaces for data access remain a practical reality for data engineers. We frequently run into situations where not all data and functionality in a SaaS platform is exposed through automated interfaces such as APIs and file drops. Instead, someone must manually access a web interface, generate a report, and download a file to a local machine. This has obvious drawbacks, such as people forgetting to run the report or having their laptop die. Where possible, choose tools and workflows that allow for automated access to data.

## Web Scraping

`Web scraping` automatically extracts data from web pages, often by combing the web page's various `HTML` elements. You might scrape ecommerce sites to extract product pricing information or scrape multiple news sites for your news aggregator. Web scraping is widespread, and you may encounter it as a data engineer. It's also a murky area where ethical and legal lines are blurry.

Here is some top-level advice to be aware of before undertaking any web-scraping project. First, ask yourself if you should be web scraping or if data is available from a third party. If your decision is to web scrape, be a good citizen. Don't inadvertently create a denial-of-service (`DoS`) attack, and don't get your IP address blocked. Understand how much traffic you generate and pace your web-crawling activities appropriately. Just because you can spin up thousands of simultaneous `Lambda` functions to scrape doesn't mean you should; excessive web scraping could lead to the disabling of your `AWS` account.

Second, be aware of the legal implications of your activities. Again, generating `DoS` attacks can entail legal consequences. Actions that violate terms of service may cause headaches for your employer or you personally.

Third, web pages constantly change their `HTML` element structure, making it tricky to keep your web scraper updated. Ask yourself, is the headache of maintaining these systems worth the effort?

Web scraping has interesting implications for the data engineering lifecycle processing stage; engineers should think about various factors at the beginning of a web-scraping project. What do you intend to do with the data? Are you just pulling required fields from the scraped `HTML` by using `Python` code and then writing these values to a database? Do you intend to maintain the complete `HTML` code of the scraped websites and process this data using a framework like `Spark`? These decisions may lead to very different architectures downstream of ingestion.

## Transfer Appliances for Data Migration
