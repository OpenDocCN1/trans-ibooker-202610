
Make sure you have a broader strategy to back up your files from your computer or cloud account in case either of those are deleted or those systems crash.

Make a habit of using these three sourcing strategies—filenames, notes, and backups—to increase the credibility and replicability of your data visualizations. In the next section, we’ll explore more ways to reduce your chances of making “bad data” errors.

## Recognize Bad Data

When your data search produces some results, another key step is to open the file, quickly scroll through the content, and look for any warning signs that it might contain “bad data.” If you fail to catch a problem in your data at an early stage, it could lead to false conclusions and diminish the credibility of all of your work. Fortunately, members of the data visualization community have shared multiple examples of problems we’ve previously encountered, to help save newer members from making the same embarrassing mistakes. One popular crowdsourced compilation by data journalists was The Quartz Guide to Bad Data. Watch out for spreadsheets containing these “bad data” warning signs:

### Missing values

If you see blank or “null” entries, does that mean data was not collected? Or maybe a respondent did not answer? If you’re unsure, find out from the data creator. Also beware when humans enter a 0 or −1 to represent a missing value without thinking about its consequences on running spreadsheet calculations, such as SUM or AVERAGE.

### Missing leading zeros

One of the zip codes for Hartford, Connecticut is 06106. If someone converts a column of zip codes to numerical data, it will strip out the leading zero and appear as 6106. Similarly, the US Census Bureau lists every place using a Federal Information Processing Standards (FIPS) code, and some of these also begin with a meaningful zero character. For example, the FIPS code for Los Angeles County, California is 037, but if someone accidentally converts a column of text to numbers, it will strip out the leading zero and convert that FIPS code to 37, which may break some functions that rely on this code being a three-digit number, or may make some people interpret it as a two-digit state code for North Carolina instead.

### 65536 rows or 255 columns

These are the maximum number of rows supported by older-style Excel spreadsheets, or columns supported by Apple Numbers spreadsheet, respectively. If your spreadsheet stops exactly at either of these limits, you probably have only partial data. As we wrote this, the BBC reported that Public Health England lost thousands of COVID-19 test results due to this row limit in older Excel spreadsheets.

### Inconsistent date formats

For example, November 3, 2020, is commonly entered in spreadsheets in the US as 11/3/2020 (month-day-year), while people in other locations around the globe commonly type it as 3/11/2020 (day-month-year). Check your source.

### Dates such as January 1st 1900, 1904, or 1970

These are default timestamps in Excel spreadsheets and Unix operating systems, which may indicate the actual date was blank or overwritten.

### Dates similar to 4 3 8 9 1

When you type March 1 during the year 2020 into Microsoft Excel, it automatically displays as 1-Mar, but is saved using Excel’s internal date system as 43891. If someone converts this column from date to text format, you’ll see Excel’s five-digit number, not the dates you’re expecting.

Other ways to review the quality of data entry in any spreadsheet column are discussed in “Sort and Filter Data” and “Summarize Data with Pivot Tables”, as well as in Chapter 6, where you will learn to create a histogram. These methods enable you to quickly inspect the range of values that appear in a column and to help you identify bad data.

Also beware of bad data due to poor geocoding, when locations have been translated into latitude and longitude coordinates that cannot be trusted (see “Geocode Addresses in Google Sheets”). For example, visualization expert Alberto Cairo describes how data appeared to show that Kansas residents viewed more online pornography than other US states. On closer examination, the internet protocol (IP) addresses of many viewers couldn’t be accurately geocoded, perhaps because they sought to maintain their privacy by using virtual private networks (VPN) to disguise their location. As a result, the geocoding tool automatically placed large numbers of users in the geographic center of the contiguous US, which happens to be in Kansas.10 Similarly, when global data is poorly geocoded, the population booms on imaginary “Null Island”, which is actually a weather buoy located in the Atlantic Ocean at the intersection of the prime meridian and the equator, where the latitude and longitude coordinates are 0,0. For these reasons, carefully inspect geocoded data for errors caused by tools that mistakenly place results in the exact center of your geography, as shown in Figure 3-4.

![](img/216890131d46798dfe1cd5f886cc608c_67_0.png)

Figure 3-4. Beware of bad geocoding that automatically places data in the geographic center of the contiguous US (in northern Kansas), or on imaginary Null Island in the Atlantic Ocean (the location of coordinates 0,0).

What should you do when you discover bad data in your project? Sometimes small issues are relatively straightforward and do not call into question the integrity of the entire dataset. Sometimes you can fix these using methods we describe in Chapter 4. Larger issues can be more problematic. Follow the source of your data stream to try to identify where the issue began. If you cannot find and fix the issue on your own, contact the data provider to ask for their input, since they should have a strong interest in improving the quality of the data. If they cannot resolve an important data issue, then you need to pause and think carefully. In this case, is it wiser to continue working with problematic data and add a cautionary note to readers, or should you stop using the dataset entirely and call attention to its underlying problem? These are not easy decisions, and you should ask for opinions from colleagues. In any case, never ignore the warning signs of bad data.

Finally, you can help prevent bad data from occurring by following key steps we’ve outlined. Give meaningful names to your data files and add source notes in a separate tab about when and where you obtained it, along with any definitions or details about what it claims to measure and how it was recorded. Explain what any blanks or null values mean, and avoid replacing those with zeros or other symbols. Always watch out for formatting issues when entering data or running calculations in spreadsheets.

In the next section, you’ll learn more questions to help you understand your data at a deeper level.

## Question Your Data

Now that you’ve found, sourced, and inspected some files, the next step is to question your data by looking more deeply than the surface level. Read the metadata, which are the notes that describe the data and its sources. Examine the contents to reflect on what is explicitly stated—or unstated—to better understand its origin, context, and limitations. You cannot program a computer to do this step for you, as it requires critical thinking to see beyond the characters and numbers appearing on your screen.

One place to start is to ask: What do the data labels really mean? and to consider the following potential issues.

### What Are Full Definitions for Abbreviated Column Headers?

Spreadsheets often contain abbreviated column headers, such as Elevation or Income. Sometimes the original software limited the number of characters that could be entered, or the people who created the header names preferred to keep them short. But was Elevation entered in meters or feet? An abbreviated data label does not answer that key question, so you’ll need to check the source notes, or if that’s not available, compare elevation data for a specific point in the dataset to a known source that includes the measurement unit. Similarly, if you’re working with US Census data, does the Income abbreviation refer to per person, per family, or per household? Also, does the value reflect the median (the midpoint in a range of numbers) or the mean (the average, calculated by adding up the sum and dividing by the number of values). Check definitions in the source notes.

### How Exactly Was the Data Collected?

For example, was Elevation for a specific location measured by a GPS unit on the ground? Or was the location geocoded on a digital map that contains elevation data? In most cases, the two methods will yield different results, and whether that matters depends on the degree of precision required in your work. Similarly, when the US Census reports data from its annual American Community Survey (ACS) estimates for Income and other variables, these are drawn from small samples of respondents for lower levels of geography, such as a census tract with roughly four thousand residents, which can generate very high margins of error. For example, it’s not uncommon to see ACS estimates for a census tract with a mean family income of $50,000—but also with a $25,000 margin of error—which tells you that the actual value is somewhere between $25,000 and $75,000. As a result, some ACS estimates for small geographic units are effectively meaningless. Check how data was recorded, and note any reported margins of error, in the source notes. See also how to create error bars in Chapter 6.

### To What Extent Is the Data Socially Constructed?

What do the data labels reveal or hide about how people defined categories in different social and political contexts, which differ across place and time? For example, we designed an interactive historical map of racial change for Hartford County, Connecticut using more than one hundred years of US Census data. But census categories for race and ethnicity shifted dramatically during those decades because people in power redefined these contested terms and reassigned people to different groups.¹ Into the 1930s, US Census officials separated “Native White” and “Foreign-born White” in reports, then combined and generally reported these as “White” in later decades. Also, US Census officials classified “Mexican” as “Other races” in 1930, then moved this group back to “White” in 1940, then reported “Puerto Rican or Spanish surname” data in 1960, followed by “Hispanic or Latino” as an ethnic category distinct from race in later decades. Finally, the Census replaced “Negro” with “Black” in 1980, and finally dropped mutually-exclusive racial categories in 2000, so that people could choose more than one. As a result, these historical changes in the social construction of race and ethnicity influenced how we designed our map to display “White” or “White alone” over time, with additional Census categories relevant to each decade shown in the pop-up window, with our explanation about these decisions in the caption and source notes.

There is no single definitive way to visualize socially constructed data when definitions change across decades. When you make choices about data, describe your thought process in the notes or companion text.

### What Aspects of the Data Remain Unclear or Uncertain?

Here’s a paradox about working with data: some of these deep questions may not be fully answerable if the data was collected by someone other than yourself, especially if that person came from a distant place or time period, or a different position in a social hierarchy. Even if you can’t fully answer these questions, don’t let that stop you from asking good questions about the origins, context, and underlying meaning of your data. Our job is to tell true and meaningful stories with data, but that process begins by clarifying what we know—and what we don’t know—about the information we’ve gathered. Sometimes we can visually depict its limitations through error bars, as you’ll learn in “Chart Design Principles”, and sometimes we need to acknowledge uncertainty in our data stories, as we’ll discuss in “Acknowledge Sources and Uncertainty”.

## Summary

This chapter reviewed two broad questions that everyone should ask during the early stages of their visualization project: Where can I find data? and What do I really know about it? We broke down both questions into more specific parts to develop your knowledge and skills in guiding questions for your search, engaging with debates over public and private data, masking and aggregating sensitive data, navigating open data repositories, sourcing data origins, recognizing bad data, and questioning your data more deeply than its surface level. Remember these lessons as you leap into the next few chapters on cleaning data and creating interactive charts and maps. We’ll come back to related issues on this topic in Chapter 14.
