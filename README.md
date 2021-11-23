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

Finally have a dataframe that can be used for analysis.  

### _Graphs in Tableau_

#### Initial Graphs:

•	Pie chart showing how many jobs per scope

•	Bar chart comparing estimated and actual project costs

https://public.tableau.com/app/profile/leslie.finlayson/viz/FinalProject_16360494630080/EstimatedandTotalCosts?publish=yes

#### Visusal data from further analysis: 

•	Bar chart showing how far off total costs are to estimated costs

•	Bar chart comparing estimated and actual project costs

•	Charts created in SQL 

https://public.tableau.com/app/profile/leslie.finlayson/viz/FinalProject_16360494630080/estimatecomparision?publish=yes


### _SQL database using PgAdmin 4_

To load the Pandas dataframe into SQL, added the folloiwng in Jupyter notebook:

<img width="349" alt="2021-11-22 (10)" src="https://user-images.githubusercontent.com/84471904/142949564-7cdd24d1-2614-4a26-9264-21fc00982930.png">

Queries (all shown on Tableau dashboard):  

• Count how many projects over the estimate and how many projects with actual costs under estimate 

• Sum of Gain_or_Loss column

• Created a new table listing jobs where loss was greater than $500 

## Machine Learning Model

I chose logistic regression because it predicts binary outcomes.  The most stripped down question of this analysis is whether Bob's job cost estimates for construction projects is less than the actual projects' costs and he is losing money.  The previously described analysis using Tableau and SQL shows that Bob is indeed losing money.  Can a logistic regression model predict this?

A great deal of data was collected during this analysis ETL process:


<img width="750" alt="2021-11-22 (14)" src="https://user-images.githubusercontent.com/84471904/142958169-8c6fc8f8-7f74-4b6d-92b0-781613024e5e.png">

Working with Bob, we narrowed the potentially most relevant variables to Estimated_Total_Cost, Project_Scope, Time_Months, Project_Location and Gain.

_Preprocessing needed:_

• To use Scikit-learn's machine learning algorithms, the text features (Project_Scope, Project_Location, and Gain) had to be converted into numbers. 

<img width="544" alt="2021-11-22 (21)" src="https://user-images.githubusercontent.com/84471904/142970187-9adb0305-8a0f-418c-aae7-33250fbf2d8b.png">

• Data scaling was also needed to prepare the data for machine learning. When features have different scales, features using larger numbers can have a disproportionate impact on the model. Scaling reduces the likelihood that large values will unduly influence the model.  In this case, Estimated_Total_Cost needed to be scaled.  

<img width="318" alt="2021-11-22 (23)" src="https://user-images.githubusercontent.com/84471904/142970612-91af28cb-5bbb-42d4-ab13-f52ca3bcb298.png">









