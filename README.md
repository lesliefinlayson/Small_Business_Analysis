# Happy To Help Construction Analysis 

## Project Overview and Purpose

Business is booming for Happy to Help Construction, a local construction company that has been specializing for over two decades in kitchen and bathroom remodels and has recently expanded to larger scale multi-scope remodels.  More and bigger projects, bigger bids, larger crews, the Company must be doing great. Bob (owner and CEO) relies on his knowledge and experience to estimate bids and relies largerly on his math skills and memory to track current projects costs, including material and labor.

Question for this analysis - how well is this working?  Are Bob's project "guesstimates" close to actual project costs?

## Analysis

### _Pre-processing_

Bob uses a hodgepodge of sources to keep up with project costs, including multiple Excel spreadsheets and a cloud-based time tracking tool (TSHeets) to track employee hours (except for one employee who submits invoices for payment).  

Task:  collect information in one place.  I used Pandas in Jupyter Notebooks for the ETL process.

Steps taken:
1.  Created dataframes for each of the Excel spreadsheets and used info() to review non-null counts and data types.  
2.  The imported csv files contain "currency" data type and had to be converted to numeric formats. 

<img width="420" alt="2021-11-21 (13)" src="https://user-images.githubusercontent.com/84471904/142788352-b2db31ee-15fd-4610-a578-5c052f4b3481.png">

3.  Set "Job" as common index when possible
 
4.  Created new dataframes to calculate actual material costs and actual labor costs

5.  Used "groupby" material costs and labor costs per job.  Example:

 <img width="397" alt="2021-11-21 (15)" src="https://user-images.githubusercontent.com/84471904/142788648-f72f5292-8c65-4c7c-a965-cb1edb635456.png">
6.  Added these new columns to the existing dataframes using "merge".  Example:

<img width="605" alt="2021-11-21 (18)" src="https://user-images.githubusercontent.com/84471904/142788905-286d96ff-5c36-4944-89c5-01546f228da9.png">

7.  To generate labor costs, created new column (Labor_Estimate) in TSheets dataframe which shows employee hourly rates, sum by job, drop unnecessary columns, and merge with actual labor costs dataframe:  

<img width="549" alt="2021-11-22 (2)" src="https://user-images.githubusercontent.com/84471904/142927587-0e5e57c7-d46b-4dc7-8e1c-49a721ff263a.png">

8.  For the one employee that does not use TSheets for tracking time (Randy), calculate his costs per job and add merge it with the actual labor costs dataframe:

<img width="618" alt="2021-11-22 (3)" src="https://user-images.githubusercontent.com/84471904/142928046-a2ad375b-6b97-49f6-9aaa-a67e7659aed6.png">

9.  Sum the two labor columns to create the actual labor costs dataframe and merge with the materials dateframe to create the final dataframe

<img width="758" alt="2021-11-22 (4)" src="https://user-images.githubusercontent.com/84471904/142929080-ed1e47e0-60ab-4932-bc8c-7c16cdcebd2c.png">

10. Sum Actual Labor costs and Actual Materials Cost for a new column Actual_Total_Cost.  Make a new column (Gain_or_Loss) showing the difference between estimated and total costs

<img width="759" alt="2021-11-22 (6)" src="https://user-images.githubusercontent.com/84471904/142929967-ed703885-2fba-426c-ac03-1382399d7710.png">

11.  Add a new column "gain" which shows "yes" if the difference between the estimated cost and the actual cost is greater than 0 and "no" if the difference between the estimated cost and the actaul cost is less than 0:  

<img width="775" alt="2021-11-22 (8)" src="https://user-images.githubusercontent.com/84471904/142930860-420b28f2-2dc1-471d-860a-ee8a4e653703.png">



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


