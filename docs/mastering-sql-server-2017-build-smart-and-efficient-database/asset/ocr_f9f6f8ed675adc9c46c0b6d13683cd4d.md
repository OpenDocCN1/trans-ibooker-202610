# Programming

Without programming, a SQL Server isn't very useful. The programming landscape of SQL Server has continued to improve to adopt newer technologies over the years. SQL Server 2017 is no exception in this area. There have been some long-awaited general improvements and also some rather revolutionary additions to the product that changes the way SQL Server can be used in future projects. This section will outline what programming improvements have been included in SQL Server 2017.

# Transact-SQL enhancements

The last major improvements in the T-SQL language allowed for better processing of running totals and other similar window functions. This was already a boon and allowed developers to replace arcane cursors with high-performance T-SQL. These improvements are never enough for the most performance conscious developers among us, and as such there were still voices requesting further incorporation of the ANSI SQL standards into the T-SQL implementation.

Notable additions to the T-SQL syntax include the ability to finally split comma-separated strings using a single function call, STRING_SPLIT(), instead of the previous hacky implementations using loops or the Common Language Runtime (CLR).

The sensible opposing syntax for splitting strings is a function to aggregate values together, STRING_AGG(), which returns a set of values in a comma-separated string. This replaces similarly hacky solutions using the XML data type of one of a multitude of looping solutions.

Each improvement in the T-SQL language further extends the toolbox that we, as developers, possess to be able to manipulate data inside SQL Server. The ANSI SQL standards provide a solid basis to work from and further additions of these standards are always welcome.

# JSON

It is quite common to meet developers outside of the Microsoft stack who look down on products from Redmond. Web developers, in particular, have been critical of the access to modern data exchange structures, or rather the lack of it. JSON has become the de facto data exchange method for the application development world. It is similar in structure to the previous cool-kid XML, but for reasons beyond the scope of this book, JSON has overtaken XML and is the expected payload for application and database communications.