# Happy To Help Construction Analysis 

## Project Overview and Purpose

Business is booming for Happy to Help Construction, a local construction company that has been specializing for over two decades in kitchen and bathroom remodels and has recently expanded to larger scale multi-scope remodels.  More and bigger projects, bigger bids, larger crews, the Company must be doing great. Bob (owner and CEO) relies on his knowledge and experience to estimate bids and relies largerly on his math skills and memory to track current projects costs, including material and labor.

Question for this analysis - how well is this working?  Are Bob's project "guesstimates" close to actual project costs?

## Analysis and Results

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

### Visualizations

## _Graphs in Tableau_

#### Initial graphs: https://public.tableau.com/app/profile/leslie.finlayson/viz/FinalProject_16360494630080/EstimatedandTotalCosts?publish=yes

#### Further analysis with graphs and charts: https://public.tableau.com/app/profile/leslie.finlayson/viz/FinalProject_16360494630080/estimatecomparision?publish=yes

•	Pie chart showing how many jobs per scope:

<img width="386" alt="2021-11-24 (2)" src="https://user-images.githubusercontent.com/84471904/143255494-6b3fcc62-68b3-47ec-98bc-6bd0f351db4a.png">

While pie charts are not the best way to convey data, this was of interest to Bob.  He was not aware that job scope had shifted from primarily kitchen and bathroom remodels to large scale remodels.  

•	Bar chart comparing estimated and actual project costs

<img width="492" alt="2021-11-24 (3)" src="https://user-images.githubusercontent.com/84471904/143256411-bb5f5dff-b191-4d12-9522-eb6d7b68e1fd.png">

This chart quickly shows how close job actual total costs are to Bob's estimated total costs.  Indicator that something is off with Bob's bids:  the green bars (actual total project costs) are quite often taller than the yellow bars (estimated project total costs). 


•	Bar chart showing how far off total costs are to estimated costs

<img width="352" alt="2021-11-24 (4)" src="https://user-images.githubusercontent.com/84471904/143258309-75304372-139f-482d-8eb6-1ebce515641a.png">

This chart very clearly shows that the bid cost estimates are off from the actual total costs.  Particularly worrisome, the dark orange bars, such as Wagner.  Clicking on this bar shows a loss of over $6000!

### _SQL database using PgAdmin 4_

To load the Pandas dataframe into SQL, added the folloiwng in Jupyter notebook:

<img width="349" alt="2021-11-22 (10)" src="https://user-images.githubusercontent.com/84471904/142949564-7cdd24d1-2614-4a26-9264-21fc00982930.png">

Queries (all shown on Tableau dashboard):  

• Count how many projects over the estimate and how many projects with actual costs under estimate 

<img width="155" alt="2021-11-24 (5)" src="https://user-images.githubusercontent.com/84471904/143259897-4de0900e-7342-4661-ba45-28b81eb3a158.png">

Interesting piece of information, more of Bob's bids over-estimated total project costs than under-estimated.

• Sum of Gain_or_Loss column

<img width="155" alt="2021-11-24 (6)" src="https://user-images.githubusercontent.com/84471904/143260289-4c6d065b-8211-4ef4-8ec2-79b134f7d341.png">

However, even though more job cost estimates were higher than actual total project estimates, when summed together there has been a loss of over $12,000.  Not good news for Bob.  

• Created a new table listing jobs where loss was greater than $500 

<img width="294" alt="2021-11-24 (7)" src="https://user-images.githubusercontent.com/84471904/143260749-ee970e38-9969-4734-9fb9-054786668e89.png">

Bob asked to see a list of projects where the estimated bid cost was under the actual total costs by $500 or more.  This chart shows 9 projects meeting this criteria.  Interestingly, all are large scale remodels.  

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

_Supervised Learning Tasks:_

• Split the data into Input (X) and output (y)

<img width="428" alt="2021-11-22 (25)" src="https://user-images.githubusercontent.com/84471904/142971054-e7014893-fbc3-49c3-9897-0426ab4a8eaf.png">

• Create an instance of the model with model = LogisticRegression().

• Train the model with the dataset with model.fit(X,y).

• Create predictions with y_pred = model.predict(X).

_Evaluating the Model:_

Confusion Matrix:

<img width="237" alt="2021-11-22 (29)" src="https://user-images.githubusercontent.com/84471904/142972699-0948f269-cae6-4a5c-be3d-557146cec178.png">

What it means:

• Model predicted 1 true that was actually true

• Model predicted 0 true that was actually false

• Model predicted 2 false that were actually true

• Model predicted 4 false that were actually false

Classification Report:

<img width="313" alt="2021-11-22 (30)" src="https://user-images.githubusercontent.com/84471904/142973029-1771a26d-5a81-43ed-a08d-3fcff0cab0e7.png">

What it means:

• Model’s prediction accuracy at 71%

• Precision for predicting project losing money (gain = 0) at 100%

• Recall (sensitivity) for predicting project loss is lower than predicting project gain, which is reflected in the dropped F1 score as well

## Interactive Features - Buttons - and Fine Tuning with Visual Basic

A great deal of data has been generated and analyzed.  Bob has requests:

• Simplify worksheet by hiding columns not currently needed and add color to the most important column - Gain

• Add a button to quickly show the final tally of the Gain_or_Loss column

<img width="462" alt="2021-11-22 (33)" src="https://user-images.githubusercontent.com/84471904/142974076-ad5d7766-83aa-4d25-83da-e5afa8c89c99.png">

Just to show Bob what else we can do, created a survey button.  If "yes" selected, message saying "Great!!" appears.  If "no" selected, a message stating "We'll keep working on it!" is displayed. 

<img width="630" alt="2021-11-22 (34)" src="https://user-images.githubusercontent.com/84471904/142973927-d73696f8-dbb4-4e76-9825-e50035459b8e.png">

### _Project Challenges_

There was a great deal of data collected in different ways.  It took some exploratory work to determine how to proceed with answering the question.

While I was able to do the ETL I wanted in Jupyter notebooks, it was repetitive and not very elegant.  I am sure that there would be ways to improve this, including more functions.

After I had done the transformation part of ETL in Jupyter notebooks - adding additional columns such as showing the difference between the estimated total and the actual total - realized it might have been better for future work to have done this in the SQL dataframe.

Machine learning model - do not think I was able to clearly show how this would be helpful to Bob.  Could use more work, perhaps use a different model.

### _Project Summary_

By using the tools I learned in this data analytics bootcamp, was very clearly able to demonstrate that Bob's bid estimates are off and that he is losing money by under-estimating costs.  This is visually demonstrated in the graphs created in Tableau and the charts generated from the SQL database.

The google slide presentation offers a quick synapsis of this project: https://docs.google.com/presentation/d/1BM0jROp515q0DkptDZwpbM5Stxlr1Xe-UMKnNxfcv6Q/edit#slide=id.p

### _Next Steps_

Dive deeper into the data.  For example, the SQL chart that lists which projects had total actual costs $500 or greater than the estimated costs were all multi-scope projects.  This could be further explored and of help to Bob.  

Set up tracking for current projects:  help Bob transition to a more formal (accurate) way to track job labor and material costs.

Bob prefers simplicity and quick answers - create more buttons so he can quickly access information.  An interactive webpage might be of value too.  





