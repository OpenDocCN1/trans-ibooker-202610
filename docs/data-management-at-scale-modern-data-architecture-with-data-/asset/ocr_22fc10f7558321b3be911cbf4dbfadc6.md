**Orchestrating services**
Composing several fine-grained services into a single higher-order composite service

**Rule-based routing**
Receiving and distributing messages based on specified conditions

**Transformation to standard message formats**
Transforming one specific data format to another (this also includes data-interchange format transformations, such as SOAP/XML to JSON)

**Mediating for flexibility**
Supporting multiple versions of the same interface for compatibility

**Adapting for connectivity**
Supporting a wide range of systems for communicating with the backend application and transforming data from the application format to the bus format

The ESB promotes agility and flexibility because it enables companies to more quickly deliver services using buses and adapters. It turns traditional applications into modern applications with easy interfaces. The ESB also decouples applications by providing abstractions and taking care of mediation. The adapter connects the applications and the ESB, and handles the protocol conversions between them.

To facilitate the implementation of the ESB, many software vendors began making their applications and platforms ready for SOA by modernizing their application designs. Standards-based service interfaces emerged based on World Wide Web Consortium (W3C) standards, such as Simple Object Access Protocol (SOAP) with Extensible Markup Language (XML) as the default messaging format.

# ## What Are WSDL, XSD, and Service Repositories?

XML-based communication is typically supported by WSDL, XSD, and a service repository. Web Services Description Language (WSDL), also written in XML, is a standard used to describe a web service and its operations. It specifies the methods, location, data types, and transport protocol. It eases the work of integration and software development because a developer can develop a web service completely by just having the WSDL provided.

WSDL documents are associated with another XML-based document, called an XSD (XML Schema Definition) document. XSD documents describe the schema, which is a definition of how XML documents are structured. They can be used to ensure that a given XML document is valid and follows the rules laid out in the schema.