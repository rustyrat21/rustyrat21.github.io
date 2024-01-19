---
title: Steps for Creating a script or query
date: 2024-1-15 21:05:00 +0500
categories: [PowerShell, MSSQL]
tags: [powershell,sql,redshift]
---

Some programmers might be able to sit down and just write a great program, script, or query.  For the rest of us, these are the steps that I follow to create a solid reusable script and/or query.

## Step 1 - Know what the Output should be
- Make sure to ask detailed questions, even if it is to yourself.
- These questions should cover as many aspects as possible, but remind the manager that is requesting the script or query that there will be more questions in the future.
- Knowing what the Output should be, doesn't mean that the programmer or data engineer is the one doing the validating, this should be done by the business.  This statement means, learn what the manager is looking for.  Do they want the output to be a CSV,  or a query that can be run by Power BI, or is this to output the data into parquet for long term archive.  Knowing what the Output should be will answer the following:
	- Who is going to use this script or query
	- Will there be other tasks that will be needed, such as a service account for Power BI to use to run the query or does the manager have the permissions already
	- What the actual expectation of the output should look like.  Column names, date spans, formulas, ...
	- This will also instruct the programmer of what data to not add to the script or query.  This is as important as knowing what data should be in the report.
	- Will this be used for Public websites or only internal.
	- These are the types of questions that will get a programmer started on the script or query, but these are not the only questions that should be asked.  There will be questions that are more geared toward what the script or query will be used for, such as, who will be validating the data, and who is signing off on the data each time after the data has been run.

## Step 2 - Break it into it's smallest parts
By breaking the script or query into its smallest parts, this will help the programmer or data engineer view each step.  This will also assist the programmer or data engineer with making sure that all the steps that are needed for this program or query are known.  This will assist them with asking more questions to the manager.  This will also point them to questions for Google.  No need to re-invent the wheel, the programmer or data engineer will be versed in how to ask Google specific questions.  Find out what does and what does not work, or if someone has already built a part of the report and how they did it.  This leads into the next step.
## Step 3 - Research, Research, Research
This step is more commonly skipped over so the script or query is given to the manager in less time, but skipping this step could cause problems down the road.  For example, if a manager askes for a report needs specific formulas, the data engineer might not be aware that next year the IT department will be upgrading their SQL instance by 2 or 3 versions.  This could break some of those formulas, as some of the methods that were used were deprecated.  If the data engineer took 20 to 30 minutes to do a little research then this would have been known during the building process, so the data can be presented to the manager but know the data engineer knows that the methods used to build the report will work in the future versions of SQL that the IT department will be upgrading to.

Not everything has been built by someone else, but by breaking down the script or query into its smallest parts, know the programmer or data engineer can Google specific questions about the data.  For example, is this the correct formula or does the vendor have an API that can be used to verify the data or get some of the data.

Do not try to research each and every step of your script. Look to see if someone has already completed the areas of the script or query that you believe will be more difficult than the others.  Remember to document where you found your information, case you need to go back at a later date or for another project.

## Step 4 - Method
### Writing a script
I look at all of the steps to the script that I have on a piece of paper or a white board.  I will start to build the script using the research that I have gathered.  Whether it’s reading in JSON or building out a silent install package for an upgrade.  Start small, and keep each step small, no need to conquer the whole world in the first 5 minutes.  Make sure that there are ways to validate each step is working properly.  This could mean either creating a log file that gets built each time the script is run or building each step independently, then merging them together in the end.

#### Reasons for building steps separately
There are valid reasons for some steps to be built separately and even stay separate.  For example, you are building out an install package that will be pushed out from Microsoft Intune.  During the conversations around the update, it is made known that an INI file needs to be modified after installation to either add, comment out, or remove text to assist with making the customer believe there were no changes. By having a separate script run after installation is complete these changes can be made and the customer doesn’t even know.  

### Writing a query
When writing a query to pull data, I find it easier to work with the tables or views that give you the most data, then understand what tables need to be joined to gather the remaining data.  Once all the data has been presented, I will work on the more specific data points such as combining fields, building out formulas, making changes to the data so all the data within the field matches (for example, first letter of the first, middle, and last name are capitalized; unless the first 3 letters are like “McH” will be corrected), removing duplications, and building out the final layout of the data.  

Once I am in the layout phase of building the data, whether a data feed or report, I prefer to make the layout flow and easy to understand and read.  If the query came with a layout, then than makes my job easier, as I start to place the fields in that layout from square one.  This helps me make sure that all the fields are represented and nothing is missed.

## Step 5 - Validation
It is my job to write the script or query, but it’s someone else’s job to validate the data.  This statement is true, but I also need to use common sense.  Does the data look accurate, does it make sense, I will ask myself once I am ready to have the data validated.  If it does, then I will pass the script or query to the business for validation.  On the other hand, if the data does not make sense, then I will ask another programmer or data engineer if the data makes sense.  

More than likely, the data doesn’t look right because it isn’t. Getting help from another programmer or data engineer, can be more helpful than going to the business directly.  

## Step 6 - Maintenance 
With queries, one can receive notifications if something goes wrong with a data feed or a report.  Unless the notifications are built into a script, this is less likely.  The business will reach out with either enhancements or issues they are seeing when the script or query runs.  These issues should be looked at with an understanding of the complete picture.  It is the business’s job to get us as much information about the issue as possible, this will stop me from going down a length rabbit hole and wasting time.  