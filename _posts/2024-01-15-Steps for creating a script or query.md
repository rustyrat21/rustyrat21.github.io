---
title: Documentation on several Microsoft OS, Platforms, and Applications
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