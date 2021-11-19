# Happy To Help Construction Analysis 

## Project Overview and Purpose

Business is booming for Happy to Help Construction, a local construction company that has been specializing for over two decades in kitchen and bathroom remodels and has recently expanded to larger scale multi-scope remodels.  More and bigger projects, bigger bids, larger crews, the Company must be doing great. Mike(owner and CEO) relies on his knowledge and experience to estimate bids and relies largerly on his math skills and memory to track current projects costs, including material and labor.

Question for this analysis - how well is this working?  Are Mike's project "guesstimates" close to actual project costs?

## Analysis

### _Pre-processing_

Mike uses a hodgepodge of sources to keep up with project costs, including multiple Excel spreadsheets and a cloud-based time tracking tool (TSHeets) to track employee hours (except for one employee who submits invoices for payment).  

Task:  collect information in one place.  I used Pandas in Jupyter Notebooks for the ETL process.

Steps taken:
1.  Created dataframes for each of the Excel spreadsheets and used info() to review non-null counts and data types.  


Week 2 - Project Notes

•	completed ETL

•	started visualizations in Tableau

[link to dashboard](https://public.tableau.com/app/profile/leslie.finlayson/viz/FinalProject_16360494630080/totals?publish=yes)

_Appears to be a trend that larger/multi-scoped projects have less accurate bids.  Something to look at in R?_

_Interactive piece of project.  Owner would like to find at a glance information when he has a question.  For example, be able to quickly search/find what work was done for a particular client._  





Week 1 - Project Notes

Selected Topic:  Happy to Help Construction Inc. – how’s the company doing?  The owner generates bids for projects, are they accurate?  Capture the costs of the actual project?  
Why:  Know the owner, he’d like to know!

Sources of data:  

•	List of construction projects since 1/2017 in Excel spreadsheet

•	Employee pay rates in Excel spreadsheet

•	Employee hours downloaded from TSheets in CVS file

•	Receipts – entered manually to Excel file

Questions hope to answer:

•	How “good” are the bid estimates (listed by project on bids.csv)? 

•	Is Mike(the owner) able to accurately estimate bids for all project types? Is there a difference by type?  Or by cost (break down by categories?)

Data exploration phase – 

•	In Jupyter Notebooks, look at each data source

o	Convert Excel files to CSV

o	Load files and create dataframes

	Column names, data types, any null values

o	Review data types – most are object and may need to be converted to integer or float

•	What information needed to answer questions?

o	Step 1:  compare actual material costs to estimated material costs/job

	Estimated material costs/job in bid.csv

	Actual material costs/job in receipts.csv.  Will need to be totaled/job

•	Compare with graph(bar)

o	Step 2:  compare actual labor costs to estimated labor costs/job

	Estimated labor costs/job in bid.csv

	Actual labor costs will need to be calculated.  Employee hours/job in TSheets.csv.  Employee hourly rate in pay.csv.  Will need to be calculated and totaled/job 

•	Compare with graph(bar)

o	Step 3:  compare total estimated costs with total actual costs/job

Machine Learning – to answer 2nd question –  not defined yet.  Maybe look at clusters – by project costs or type.  


