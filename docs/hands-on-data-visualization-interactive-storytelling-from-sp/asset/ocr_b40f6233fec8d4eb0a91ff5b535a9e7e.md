# Combine Data into One Column

Let’s perform the reverse action by combining data into one column with a spreadsheet formula, also called concatenation, using the ampersand symbol (&). Imagine you receive address data in four separate columns: street address, city, state, and zip code:

| Street | City | State | Zip |
|---|---|---|---|
| 300 Summit St | Hartford | CT | 06106 |

But imagine you need to geocode the addresses using a tool like the one we introduced in “Geocode Addresses in Google Sheets”, which requires all of the data to be combined into one column like this:

| Location |
|---|
| 300 Summit St, Hartford, CT 06106 |

Using any spreadsheet, you can write a simple formula to combine (or concatenate) terms using the ampersand (&) symbol. Also, you can add separators into your formula, such as quoted space (" "), or spaces with commas (", "), or any combination of characters. Let’s try it with some sample data:

+   1. Open the **Combine Separate Columns** sample data in Google Sheets, sign in with your account, and go to File > “Make a copy” to create a version you can edit in your Google Drive. The sheet contains addresses that are separated into four columns: street, city, state, and zip.
+   
+   2. In column E, type a new header named *location*.
+   
+   3. In cell E2, type `=A2 & ", " & B2 & ", " & C2 & " " & D2`. This formula combines the four items using ampersands, and separates them with quoted commas and spaces. Then press Enter.
+   
+   4. Click cell E2 and drag the bottom-right corner crosshair downward to fill in the rest of the column.

![](bbox=[0.222, 0.443, 0.902, 0.685])

Now that you have successfully combined the terms into one location column, you can use the Geocoding by SmartMonkey Google Sheets add-on we described in “Geocode Addresses in Google Sheets” to find the latitude and longitude coordinates in order to map your data, as we’ll discuss in Chapter 7.

For further reading, we recommend Lisa Charlotte Rost’s brilliant Datawrapper blog post about cleaning and preparing spreadsheet data for analysis and visualization.²

Spreadsheets are great tools to find and replace data, split data into separate columns, or combine data into one column. But what if your data table is trapped inside a PDF? In the next section, we will introduce Tabula and show you how to convert tables from text-based PDF documents into tables that you can analyze in spreadsheets.

# Extract Tables from PDFs with Tabula

It sometimes happens that the dataset you’re interested in is available only as a PDF document. Don’t despair—you can likely use Tabula to extract tables and save them as CSV files. Keep in mind that PDFs generally come in two flavors: text-based and image-based. If you can use the cursor to select and copy and paste text in your PDF, then it’s text-based, which is great because you can process it with Tabula; if you can’t, then it’s image-based, meaning it was probably created as a scanned version of the original document. You need to use optical character recognition (OCR) software, such as Adobe Acrobat Pro