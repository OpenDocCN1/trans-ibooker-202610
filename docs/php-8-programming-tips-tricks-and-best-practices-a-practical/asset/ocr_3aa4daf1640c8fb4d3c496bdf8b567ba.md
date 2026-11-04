If you were to use this class as part of a Doctrine ORM implementation, Doctrine would open the file and parse the DocBlocks, searching for @ORM annotations. Despite some concerns over the time and resources needed to parse DocBlocks, this is an extremely convenient way to define the relationship between object properties and database table columns, and is popular with developers who use Doctrine.

> **Tip**
> Doctrine offers a number of alternatives to this form of ORM, including **Extensible Markup Language (XML)** and native PHP arrays. For more information, see https://www.doctrine-project.org/projects/doctrine-orm/en/latest/reference/annotations-reference.html#annotations-reference.

# Hidden dangers associated with the misuse of DocBlocks

There is yet another danger associated with this abuse of the original purpose of a DocBlock. In the php.ini file, there is a setting named opcache.save_comments. If disabled, this would cause the OpCode cache engine (OPcache) to ignore all comments, including DocBlocks. If this setting is in effect, a Doctrine-based application using @ORM annotations in DocBlocks would malfunction.

Another problem has to do with how comments are parsed—or, more to the point, how comments are not parsed. In order to use the contents of a comment, the PHP application needs to open the file and parse it line by line. This is an expensive process in terms of time and resource utilization.

# The Attribute class

In order to address hidden dangers, in PHP 8 a new Attribute class is provided. Instead of using DocBlocks with annotations, developers can define the equivalent in the form of attributes. An advantage of using attributes rather than DocBlocks is that they are a formal part of the language and are thus tokenized and compiled along with the rest of your code.