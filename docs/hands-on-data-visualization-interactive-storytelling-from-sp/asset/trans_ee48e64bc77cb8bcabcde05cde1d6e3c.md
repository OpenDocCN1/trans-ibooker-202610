

2. “Drilling into the DEA’s Pain Pill Database” (*Washington Post*, July 16, 2019), https://oreil.ly/Xx7dh.
3. Jack Dougherty et al., “School Choice in Suburbia: Test Scores, Race, and Housing Markets,” *American Journal of Education* 115, no. 4 (August 2009): 523–548, https://oreil.ly/T2I81.
4. Katy Rossiter, “What Are Census Blocks?” US Census Bureau, July 11, 2011, https://oreil.ly/UTxpk.
5. D’Ignazio and Klein, *Data Feminism*.
6. Shoshana Zuboff, *The Age of Surveillance Capitalism: The Fight for a Human Future at the New Frontier of Power* (PublicAffairs, 2019).
7. Geoffrey A. Fowler, “Goodbye, Chrome: Google’s Web Browser Has Become Spy Software,” *The Washington Post*, June 21, 2019, https://oreil.ly/ef8H.
8. Geoffrey A. Fowler, “Alexa Has Been Eavesdropping on You This Whole Time,” *The Washington Post*, May 6, 2019, https://oreil.ly/eR6RG.
9. Geoffrey A. Fowler, “Facebook Will Now Show You Exactly How It Stalks You — Even When You’re Not Using Facebook,” *The Washington Post*, January 28, 2020, https://oreil.ly/ymv9T.
10. Cairo, *How Charts Lie*, 2019, pp. 99–102.
11. For a deeper analysis, see Margo J. Anderson, *The American Census: A Social History*, 2nd edition (Yale University Press, 2015).

## Chapter 4. Clean Up Messy Data

More often than not, datasets will be messy and hard to visualize right away. They will have missing values, dates in different formats, text in numeric-only columns, multiple items in the same columns, various spellings of the same name, and other unexpected things. See Figure 4-1 for inspiration. Don’t be surprised if you find yourself spending more time cleaning up data than you do analyzing and visualizing it.

| Year | City | Amount |
|---|---|---|
| 1990 | New York City | $1,123,456.00 |
| 1995-96 |  | 2.2 mil |
| 2000’s | NYC | No data |
| 2020 | New_York | 5000000+ |

*Figure 4-1. More often than not, raw data looks messy.*

In this chapter, you’ll learn about different tools to help you make decisions about which one to use to clean up your data efficiently. We’ll start with basic cleanup methods using Google Sheets in “Smart Cleanup with Google Sheets”, “Find and Replace with Blank”, “Transpose Rows and Columns”, “Split Data into Separate Columns”, and “Combine Data into One Column”. While we feature Google Sheets in our examples, many of these principles (and in some cases, the same formulas) apply to Microsoft Excel, LibreOffice Calc, Mac’s Numbers, or other spreadsheet packages. Next, you’ll learn how to extract table data from text-based PDF documents with Tabula, a free tool used by data journalists and researchers worldwide to analyze spending data, health reports, and all sorts of other datasets that get trapped in PDFs (see “Extract Tables from PDFs with Tabula”). Finally, we will introduce OpenRefine in “Clean Data with OpenRefine”, a powerful and versatile tool to clean up the messiest spreadsheets, such as those containing dozens of different spellings of the same name.

### Smart Cleanup with Google Sheets

One of the newest reasons to work with your data in Google Sheets is to use its Smart Cleanup feature, which helps identify and suggest corrections for inaccurate data. The tool opens a sidebar menu that spots potential problems, and you decide whether to accept its suggestion.

Learn what types of issues Smart Cleanup catches, and which ones it misses, using our sample data on the 10 most populated nations in the world, which contains some problems that we intentionally added:

1. Open the Smart Cleanup sample data file in Google Sheets, sign in with your account, and go to File > “Make a copy” to create a version you can edit in your Google Drive.
2. Go to Data > “Cleanup suggestions” and view items that appear in the sidebar.

The Smart Cleanup feature successfully caught a duplicate entry (row 12), and white spaces in cells A4 and A5. Click the green Remove and “Trim all” buttons to confirm that Google Sheets should clean them up.

Can you spot these other errors that Smart Cleanup missed?

- In cell A10, Russsia is misspelled with an extra s.
- In cell C6, Pakistan’s share of the world population appears in decimal form, not percentage.
- In cell D4, the US date appears in a format unlike the other entries. If you’re familiar with different international date formats, you’ll also wonder if 12/10/2020 is meant to be MM/DD/YYYY format that’s commonly used in the US, or the DD/MM/YYYY format that’s commonly used elsewhere. Smart Cleanup cannot answer this for you.

The Google Sheets Smart Cleanup feature is a good place to start. If your data is really messy, you may need to turn to more sophisticated tools described later in this chapter, such as “Clean Data with OpenRefine”. In the next section, you’ll learn another cleanup method that works in any spreadsheet: find and replace with a blank entry.

### Find and Replace with Blank

One of the simplest and most powerful cleanup tools inside every spreadsheet is the “Find and Replace” command. You can also use it to bulk-change different spellings of the same name, such as shortening a country’s name (from Republic of India to India), expanding a name (from US to United States), or translating names (from Italy to Italia). Also, you can use find and replace with a blank entry to remove units of measurement that sometimes reside in the same cells as the numbers (such as changing 321 kg to 321).

Let’s look at “Find and Replace” in practice. A common problem with US Census data is that geographic names contain unnecessary words. For example, when you download data on the population of Connecticut towns, the location column will contain the word “town” after every name:

```
Hartford town
New Haven town
Stamford town
```

Usually you want a clean list of towns, either to display in a chart or to merge with another dataset, like this:

```
Hartford
New Haven
Stamford
```

Let’s use “Find and Replace” on a sample US Census file we downloaded with 169 Connecticut town names and their populations to remove the unwanted “town” label after each place-name:

1. Open the CT Town Geonames file in Google Sheets, sign in with your account, and go to File > “Make a copy” to create a version you can edit in your Google Drive.
2. Select the column you want to modify by clicking its header. If you don’t select a column, you will be searching and replacing in the entire spreadsheet.
3. In the Edit menu, choose “Find and replace.” You’ll see the following.

```
Find and replace

Make sure it begins with a space character

Find
 town

Replace with

Search
This sheet

Match case
Match entire cell contents
Search using regular expressions Help
Also search within formulas

Find
Replace
Replace all
Done
```

| Geo_NAME | Pop2010 |
|---|---|
| Andover town | 3303 |
| Ansonia town | 19249 |
| Ashford town | 4317 |
| Avon town | 18098 |
| Barkhamsted town | 3799 |
| Beacon Falls town | 6049 |
| Berlin town | 19866 |
| Bethany town | 5563 |
| Bethel town | 18584 |
| Bethlehem town | 3607 |
| Bloomfield town | 20486 |
| Bolton town | 4980 |
| Bozrah town | 2627 |
| Branford town | 28026 |
| Bridgeport town | 144229 |
| Bridgewater town | 1727 |
| Bristol town | 60477 |
| Brookfield town | 16452 |
| Brooklyn town | 8210 |
| Burlington town | 9301 |
| Canaan town | 1234 |
| Canterbury town | 5132 |
| Canton town | 10292 |

4. In the Find field, type *town*, and be sure to *insert a blank space* before the word. If you do not insert a space, you’ll accidentally remove *town* from places such as *Middletown*. Also, you’ll accidentally create trailing spaces, or whitespace at the end of a line without any other characters following it, which can cause trouble in the future.
5. Leave the “Replace with” field blank. Don’t insert a space. Just leave it empty.
6. The Search field should be set to the range you selected in step 2, or “All sheets” if you didn’t select anything.
7. You have the option to “Match case.” If checked, *town* and *Town* and *tOwN* will be treated differently. For our purpose, you can leave “Match case” unchecked.
8. Press “Replace all.” Because this sample file contains 169 towns, the window will state that 169 instances of “town” have been replaced.
9. Inspect the resulting sheet. Make sure that places that include *town* in their name, such as *Middletown*, remained untouched.

### Transpose Rows and Columns

Sometimes you download good data, but your visualization tool requires you to transpose, or swap, the rows and the columns to create the chart or map you desire. This problem often arises when working with time-series or historical data because they are treated in opposite ways in tables and charts. When designing a table, the proper method is to place dates horizontally as column headers, so that we read them from left to right, like this:

| Year | 2000 | 2010 | 2020 |
|---|---|---|---|
| Series1 | 333 | 444 | 555 |
| Series2 | 777 | 888 | 999 |

When designing a line chart in Google Sheets and similar tools, which you’ll learn about in **Chapter 6**, we need to transpose the data so that dates run vertically down the first column, so the software reads them as labels for a data series, like this:

| Year | Series1 | Series2 |
|---|---|---|
| 2000 | 333 | 777 |
| 2010 | 444 | 888 |
| 2020 | 555 | 999 |

Learn how to transpose rows and columns in our sample data:

1. Open the *Transpose sample data file* in Google Sheets, sign in with your account, and go to File > “Make a copy” to create a version you can edit in your Google Drive.
2. Select all of the rows and columns you wish to transpose, and go to Edit > Copy.
3. Scroll farther down the spreadsheet and click on a cell, or open a new spreadsheet tab, and go to Edit > “Paste special” > “Paste transposed.”

