Today, here on earth, Licklider’s thought experiment on how to communicate with aliens is at the heart of making RESTful Web Microservices (RWM) a reality. As we work to design and implement services that communicate with each other on the web, we, too, need to adopt a “meta-message” approach. This is especially important when we consider that one of the aims of our work is to “get communications started among totally uncorrelated” services. In the spirit of our guiding principle (see Recipe 1.3), people should should be able to confidently design and build services that will be able to talk to other services built by other people they have never met whether the services were built yesterday, today, or in the future.

The recipes in this section are all aimed at making it possible to implement “Licklider-level” services. You’ll see this especially in sections on *design negotiation*, and *design bending*.

# Morville’s Information Architecture

The 1990s was a heady time for proponents of the Internet. Tim Berners-Lee’s World Wide Web and HTTP/HTML (see “The Web of Tim Berners-Lee”) was up and running, Roy Fielding was defining his REST architecture style (see “Fielding’s REST”) and Richard Saul Wurman ¹⁴ was coining a new term: “Information Architect”. In his 1996 book “Information Architects” ¹⁵ Wurman offers this definition:

> *Information Architect: 1) the individual who organizes the patterns inherent in data, making the complex clear; 2) a person who creates the structure or map of information which allows others to find their personal paths to knowledge; 3) the emerging 21st century professional occupation addressing the needs of the age focused upon clarity, human understanding and the science of the organization of information.*

> *Richard Saul Wurman, 1996*

A physical architect by training, Wurman founded the Technology, Entertainment, and Design (TED) conferences in 1984. A prolific writer, he has penned almost 100 books on all sorts of topics including art, travel, and