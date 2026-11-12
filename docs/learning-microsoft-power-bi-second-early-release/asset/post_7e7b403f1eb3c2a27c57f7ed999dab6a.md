
The Move button allows you to move the location of a column in a table. You can move it left, right, to the beginning, or to the end. You can do this with multiple columns selected. Rename allows you to rename a chosen column.

Pivot Column allows you to take the values of a column, turn those values into columns, and recalculate values across the new combination. This can be useful when you have attributes in one column and values in another, and you want to see the values with the attributes as columns.

Convert to List is a unique function. It takes one column and turns it into a special type of table in Power Query called a List. Lists can be passed sort of like parameters to special custom functions. We're not going to worry too much about lists in this book.

The Text Column section is a bit strangely named because you can do these functions on more than just text columns. Thankfully, they are pretty straightforward for the most part.

Split Column allows you to take a column and split it into two columns based on a delimiter selection. Format does not allow you to edit the data type. It does allow you to do things like making all the data lowercase or uppercase or adding prefixes or suffixes. Note that using this feature will turn that column into a text type column. Merge Columns smashes two or more columns together, allowing you to choose a separator. Extract allows you to modify a column or columns to keep specific characters in a column. I don't use this very often, but when I have, I used it mostly to remove non-Unicode characters.

Parse does something different compared to everything else here. Parse takes a JSON or XML file, semistructured data, and puts it into a more classically structured format for analysis. We aren't using any JSON or XML files in our examples, but if you were working with NoSQL databases or getting results from API calls, you might need to parse that data before you could do analysis on it, and this parse function does that.

The Number Column functions can allow you to get statistics on a column as a separate value, perform standard calculations against a column to edit its values, apply scientific calculations against a column, perform trig functions against a column, round a column, or find whether a value is even or odd or its sign. With the exception of the statistics function, I recommend duplicating a column before applying any modifications to the data; then you can have both the original clean value and the modified value. The Date & Time area functions in the same way as the Number Columns, with numerous date and time transformations.

The final section in the Transform tab is the Scripts section, which offers the ability to run both R and Python scripts against your data. In order to use these functions, you must have R or Python installed, and Power BI has to know where your installation of those language libraries are. If you're more comfortable working with data frames in R or Python and would rather perform transformations there and use that as a single step, you can. You can also do pre-prep of the work in Power Query, then apply a script task, and then make further modifications in Power Query. You can mix and match these functions, and if you wanted to use both, you could. Learning R and Python is beyond the scope of this book, but if you are going to pursue a career in data analytics, both of these languages could prove very useful to you.

## The Add Column Tab

First thing to note in the Add Column tab is that except for the General section, we have seen all the other functions in other places in the ribbon to this point. However, the General section contains so much that it still merits a section of its own. Looking at Figure 3-9 will make it a little more clear.

![](img/56e23132d5da9dafb7dc28cda3649f95_83_0.png)

Figure 3-9. The Power BI Ribbon will often have multiple places to find certain functionality, but at least they're topical.

Columns from Example is actual, factual, black magic. You can select a column or columns to get pieces of data from. Then you'll have to give some examples of what the response would be, and it will attempt to autofill the column as you provide examples to work from. In our Grades Query, for example, I'm going to add a column from examples that will say if the Office Hours Attended column is null, then no, but otherwise yes. We can see this in Figure 3-10, and you also have the bonus of seeing the logic it put together in M, which can be another way to see how the language handles different things.

![](img/56e23132d5da9dafb7dc28cda3649f95_84_0.png)

Figure 3-10. A couple of examples of what I want, sprinkle in some AI, and viola, fresh column!

Custom Column will bring up a dialogue menu where you can create a custom column using M. This can be useful if you have transformations that you want to do that maybe aren't supported by the UI or that you could do more efficiently via code. The window will at least tell you if there is a syntax error in the code, but if you're just learning M, the errors aren't always super helpful.

Invoke Custom Function is an incredibly powerful tool that allows you to take a function and iterate over that function for a given column, finding a specific value, and then applying some sort of logic. A simple example would be creating a function that would make a new column that would take the value of one column and multiply it by another to get a third value that would be placed in this new column. Did you follow that logic? This is a very powerful tool, but it can also be hard to wrap your head around what it does the first time you work through it.

Conditional Column allows you to create a column along the lines of an if-then-else statement. If Column X is less than Column Y, then Yes, else No. You can also have multiple clauses for more complicated or iterative if functions. If you're familiar with SQL, think of this much like a case when statement. Figure 3-11 shows us the dialogue window for adding a Conditional Column.

![](img/56e23132d5da9dafb7dc28cda3649f95_85_0.png)

Figure 3-11. Sometimes we just need an if statement

The Index Column creates a column that will either start from 0 or 1 and create a unique list of values usually corresponding to the row number. The most important thing about an index column is that it is completely unique and can be used as an impromptu key value if necessary, possibly merging that column with others to pass the index or key value to other tables for the purpose of creating relationships. Ideally, your data already has keys in place, and an index isn't necessary. But if you do need to create an index, it's very simple. Duplicate Column takes the current column and makes a copy of it. That's it. No witty commentary on that one. It's just copy and paste.

## The Model View

Returning back to Power BI, let's take a look at that third view in Power BI Desktop, the Model view. Why do relationships matter? What do they actually accomplish? Can I just ignore relationships and make one big table that can solve world hunger? We'll address these ideas in this section.

Let's take a quick look at the Model view of our current file in Figure 3-12. By default, Power BI will try to detect relationships when importing data and when the data is put together in such a way that Power BI will detect the relationships correctly. Unfortunately, it doesn't always do a good job of this. As a result, you should always double check your relationships after you import data to make sure your model is constructed as you intended.

![](img/56e23132d5da9dafb7dc28cda3649f95_86_0.png)

Figure 3-12. That's it, our data model

## What is a Relationship?

A relationship is, in the simplest terms, a way to tell Power BI what tables are connected and how. A relationship should be constructed so that the values in one column in one table match those values in another table, such that if we tried to query them, we could get back sensible results. A relationship is a combination of a few things, columns, direction, and cardinality. If we hover over the lines in our data model, they will glow yellow, and if we double click on that, we can see the relationship dialogue window as shown in Figure 3-13. In this case, I'm demonstrating the relationship between AssignmentDIM and Grades.
