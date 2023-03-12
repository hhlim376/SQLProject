# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals

  
To Load, Clean, Transform and Analyze Data from a dataset using PostgreSQL   
&nbsp;

  
## Process

  Step1: Load .csv files into a newly created "ecommerce" database in PGAdmin4  
-Create tables within the "ecommerce database"  
-Import .csv files into tables  
  
  Step 2: Clean Data  
  -Get familiar with the data, identify any issues that need to be cleaned  
  -Clean Data --> see cleaning_data.md file  
    
  Step 3: Write Queries to answer questions  
  -See starting_with_questions.md

  Step4: Think of other questions we could answer using data from this database  

  -See starting_with_data.md

  Step 5: Develop and execute a QA process to try validate the results  

  -See QA.md

  Step 6: Push completed work to github  
&nbsp;


## Results
(fill in what you discovered this data could tell you and how you used the data to answer those questions)    
&nbsp;  


This data contained customer, sales and website information for what looks to be an ecommerce business. Some things this data could tell us:
- Breakdown of which country/city the customerbase is from --> Look at country/city data for customers who ordered a product
- What products/product cateogory is the most popular in different parts of the world --> Compare sales data from different countries
- Which region had the highest spenders? --> Look at revenue data for visitors of different countries and cities
- Which year had the most site visits --> Count unique visitors to the site based on date
-  Which region had customers who spent the most time on the site --> look at time on site data for customers from different regions

For more questions, see --> starting_with_questions.md and starting_with_data.md



## Challenges 
(discuss challenges you faced in the project)  
One challenge was figuring out how to properly import the .csv files into PGAdmin4. The problem for me was some columns in the table i created did not match the right data type in the .csv file (e.g. integer when it should have been numeric).  

Perhaps the biggest challenge overall was trying to make sense of the data, trying to figure out which column contained the relevant data needed to answer the questions that were posed. 

&nbsp;

## Future Goals
(what would you do if you had more time?)  
- Refine the QA process
- Clean Data more thouroughly
- Try to extract more relevant data/trends from the dataset
  
