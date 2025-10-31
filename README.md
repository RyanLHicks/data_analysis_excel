# Excel Data Analysis
## Introduction
### Background
- This is part of a 4 part learning series where I try and learn the best data analysis and extraction tools that are currently available. My goal is to learn Python, SQL, Power BI, and Excel to fully broaden my understanding of the tools and understanding the real life applications each carry.
- I want to enhance my already learned excel skills by learning, DAX, Power Pivot, Power Query, Pivot Tables, and advanced formulas within excel. I am using a data jobs data set that is publically sourced to analyze and hone in to what skills pay the most within certain job titles and locations around the world.
  
### Goals
- Build a jobs data dashboard to digest the landscape of the data analytics field.
- Transform insights using visualization tools such as charts, graphs, and tables.
- Use advanced formulas and excel languages to dive deep into specefic questions.

### Overview
- I will first import the publically available information from data analytic jobs from around the world with in csv format.
- Dive deep into the data with complex LOOKUP, COUNT, IF, ISNUMBER, and MEDIAN functions
- Visualize the data into introductory graphs that links the job title, job country, and job type into one easy to filter section
- Use the same data and import into a new book that uses more advanced excel functionalities such as, Power Pivot, DAX, and Power Query to better analyze the dataset.
- Bring it all together with descriptive charts, line graphs and tables to understand the pay associated with popular data analytic skills.

## Analysis
### Introductory Data Jobs Dashboard
![EXCEL_bh8kqIsHW8](https://github.com/user-attachments/assets/80ea0ac0-6181-4459-978c-b2704376500a)

#### Data Tab: I imported the following data first into my excel workbook an created it in table format.
<img width="1814" height="598" alt="image" src="https://github.com/user-attachments/assets/91c06708-f22a-418a-8b98-31c8522a76a0" />

#### Data Validation Tab: After importing the data I found each unique job title, job country, and job schedule type using various functions within excel.
<img width="1813" height="796" alt="image" src="https://github.com/user-attachments/assets/dddd7f07-db9b-48c5-887b-5b5cc20df08d" />

- The function to find the unique values was simple, taking the job title short column in the data section. I then used the following function to count the job titles across the job countries, schedule, and salary:
*Note - I first used a generic =count function then after devising the other formulas for job country, salary, and type I added in the rest of the formula*
:
```
=UNIQUE(jobs[job_title_short

and

=COUNT(
IF(
(jobs[job_country]=country)*
(jobs[job_title_short]=A2)*
(ISNUMBER(SEARCH(type,jobs[job_schedule_type]))),
jobs[salary_year_avg]))
```
- Results:

<img width="313" height="213" alt="image" src="https://github.com/user-attachments/assets/7b2f4d6e-72b1-4c12-b831-549e9034a73a" />

- After that I used the =SORT function to clean up the data sorting from largest to smallest
```
=SORT(A2:B11,2,-1,)
```
- Results:

<img width="213" height="220" alt="image" src="https://github.com/user-attachments/assets/a515a375-3199-462b-bae7-98f3d8d658e0" />

- Moving on to the unique job countries I used this and sorted them from A to Z:
```
=UNIQUE(jobs[job_country])

and

=SORT(H2#)
```
- Results:

<img width="254" height="759" alt="image" src="https://github.com/user-attachments/assets/326cb237-463d-4ec0-8f35-f8d7b1c4c455" />

- Finally looking at the job schedule I used the following function to find the job schedules and then sorting them to only find Full-Time, Contractor, Part-Time, Internship, and Temp Work positions because a lot of them pertain to this same schedule in combonation with another schedule type (I filtered out the array to not equal 'and' and '0' :
```
=UNIQUE(jobs[job_schedule_type])

and

=FILTER(K2#,NOT(ISNUMBER(SEARCH("and",K2#)))*(K2#<>0))
```
- Results:

<img width="433" height="556" alt="image" src="https://github.com/user-attachments/assets/da8597e2-fcfc-4bfb-8375-4b2b47ef9f85" />

- Interpretations: This is used as refrence point and will be linked to other tabs that will further break down each section including job title, country, salary, type, and platform. Really a starting point to use a 

#### Median Salary Tab
- After transfering over the unique job titles from the data validation tab, I took the median salary of the job titles and sorted them from least to greatest using the following formulas:
```
=MEDIAN(
  IF(
    (jobs[job_title_short]=A3)*
    (jobs[salary_year_avg]<>0),
    jobs[salary_year_avg]
  )
)

and

=SORT(A2#:B2#,2,1)
```
Results:

<img width="546" height="223" alt="image" src="https://github.com/user-attachments/assets/3d8979ee-756c-4401-a9ee-116f0148b455" />

Then I used the formula to define where we are taking the salaries from to use on our final dashboard to sort based on a specefic job title.
```
=XLOOKUP(title,D2:D11,E2:E11)
```
Results:

<img width="84" height="27" alt="image" src="https://github.com/user-attachments/assets/105bfd60-d0c8-46dc-9376-daa4de2139c8" />

Interpretation: This expanded the details for earnings that each job title, country, and schedule may pay for the main dashboard. 

#### Job Country Tab
- For this section I again transferred over data from the data validation tab with the country information and then created a formula to find the median formula for each country:
```
=MEDIAN(
IF(
(jobs[job_country]=A3)*
(jobs[salary_year_avg]<>0)*
(jobs[job_title_short]=title)*
(ISNUMBER(SEARCH(type,jobs[job_schedule_type]))),
jobs[salary_year_avg]))
and
=SORT(FILTER(A2:B112,ISNUMBER(B2:B112)),2,-1)
```
I used a if statement to filter through my specified parameters that I want to filter specefically the job country not equaling 0, the job title using the full title and the job schedule type. The results show an error in some of the countries but that just indicates that those countries haven't listed those positions
 
Results:

<img width="743" height="693" alt="image" src="https://github.com/user-attachments/assets/bf79e6c7-0a33-4dc0-8d5e-89a343150033" />

Interpretation: Depending on what job is selected in the main tab, you will be able to find the detailed list of job countries and job titles with the associated median salaries that support each. 

#### Job Title Tab
- This section again piggybacks off of the data valadation tab and first imports the job titles then finds the median salary for each using the following formula
```
=MEDIAN(
  IF(
    (jobs[job_title_short]=A5)*
    (jobs[salary_year_avg]<>0)*
(jobs[job_country]=country)*
(ISNUMBER(SEARCH(type,jobs[job_schedule_type]))),
jobs[salary_year_avg]
  )
)
and
=SORT(FILTER(A2:B11,ISNUMBER(B2:B11)),2,1)
and
=H2=IF($D2<>title,$E2,NA())
```
After running the formula that refrences the job title, making sure that the salary does not equal 0, searches the job shchedule type and finds everything that is true along with the median salary for a specefic job title.

Results:

<img width="1196" height="298" alt="image" src="https://github.com/user-attachments/assets/c72e3ef8-9eb0-4457-972b-2105d18a5e76" />

Interpretation: This is plain and simple finding the median salary for each job title depending on only the titles for this section. Will be linked to the main tab for ease of use, showing that senior data scientists pay the most on average and regular data analysts pay the least.

#### Job Schedule Tab
- This section focused soley on the job schedule that is Full-Time, Contractor, Part-Time, Internship, or Temp Work and here is the excel formula which is similar in format to the previous one's to align all together in the dashboard:
```
=MEDIAN(IF
((jobs[job_title_short]=title)*
(jobs[salary_year_avg]<>0)*
(jobs[job_country]=country)*
(ISNUMBER(SEARCH(A1,jobs[job_schedule_type]))),
jobs[salary_year_avg]))
and
=SORT(FILTER(A1:B5,ISNUMBER(B1:B5)),2,1)
and
=IF($D1<>type,$E1,NA())
```

This strictly finds how each job schedule type salary compares to each other and sorts them . 

Results:

<img width="1019" height="301" alt="image" src="https://github.com/user-attachments/assets/92d24f78-0fb1-4bbc-b9ae-d247c85b6bd3" />

Interpretation: Just seeing that full-time positions pay the most on average and internships pay the least.

#### Job Platform 
- This section focuses on various job searching platforms and uncovers which platforms have the most activity when considering a specefic job title and will also be linked to the main jobs dashboard for quick visualizaiton. Instead of using a IF statement we use a COUNT statement but everything else is similar:
```
=COUNT(
IF(
(jobs[job_country]=country)*
(jobs[job_title_short]=title)*
(ISNUMBER(SEARCH(type,jobs[job_schedule_type])))*
(jobs[job_via]=A2),
jobs[salary_year_avg]))
and
=SORT(A2:B594,2,-1)
```

Results:

<img width="922" height="707" alt="image" src="https://github.com/user-attachments/assets/4b45cbb0-869f-4622-8754-36419718abe8" />

Interpretation: After entering the formulas to find how common wach job platform is, I sorted them from largest to smalles and then graphed it to find the linkedin and indeed were the most commonly posted for that specefic job title selected in the main tab.

### Deep Dive into Data Jobs and Skills Pay Information  
- This section is more specefic and intentional with using power pivot to determine skills associated with jobs. This helped me use a new feature to me that I didn't know existed until this project and I can really see the potential use cases for this tool. After importing all the data provided the power pivot tool I used automatically created this data sheet for this excel document for easy manipulation of desired skills.
![EXCEL_pyzVZIvS7O](https://github.com/user-attachments/assets/84e16334-7d15-492e-8914-56e394a0864d)

#### Jobs Skills Salary
- Using the power pivot function and the data I imported, I created a pivot table for ease of customization and use to look at the count of jobs for any given skills.
![EXCEL_YYAYEv8nrT](https://github.com/user-attachments/assets/bc13010a-00b6-453b-84d3-c630bc3d9daa)

- Field Organization:
I organized the count of all the jobs in the values section and the job skills titles in the rows to count all of the jobs specefically within the pivot table from the power pivot data. I also created two slicer to find out where they are in the world and what specefic title is associated with skill count for data jobs.

<img width="353" height="769" alt="image" src="https://github.com/user-attachments/assets/438b92b8-5e14-4987-ad5f-4877aa37b002" />

<img width="448" height="265" alt="image" src="https://github.com/user-attachments/assets/26f179c2-8510-4f9a-acb0-f53758ce75d2" />

- Intrepretation: This allows easy visualization between job count and the job skills against customizable job titles and the country they are located in. Using no job title filters or country filter, we can see that SQL, Python, and Tableau are among the top desired skills within the data analyst field.

#### Job Skills Availability
- For this section we can dive deeper into how much each data job might expect for pay and find the number of skills needed on average for a specefic job title. 
 ![EXCEL_9l6B7sgSMg](https://github.com/user-attachments/assets/a782b8aa-7d92-4bdb-88b8-4d3a6f739961)

To do this I first created 2 custom functions within power pivot and they are as follows:
```
med salary:=CALCULATE([median salary], CROSSFILTER(data_jobs_salary[job_id],data_jobs_skills[job_id],Both))
and
skills per job:=DIVIDE([skill count], [job count])
```
Then I inserted these custom functions into the field list for the pivot table along with the job title to find out what is the median pay and skills for each job:

<img width="347" height="759" alt="image" src="https://github.com/user-attachments/assets/530235fd-346f-4772-a3dd-8c3485fca612" />

- Interpretation: This customizable section dives deep on how many skills are required for a given job title and allows the ability to hone in on certain countries. Along with the scatter plot we are able to see how much a job might pay and how many skills are required for that given job. For example a Senior Data Engineer may expect to be paid around $155,000 but may need around 8 learned skills.

#### Job Skills Countries
- The following section looks specefically at the median salary within the entire world, the United States and non US states to grab a good picture of what to expect as a US resident comparatively

![EXCEL_bvsMsX66CI](https://github.com/user-attachments/assets/d188ac14-1c5f-421a-a4b9-c25e9f280787)

To do this I created 3 new value fields in the forms of functions for the entire median salary worldwide, in the USA specefically, and any other desired country for easy comparission:
```
median salary:=MEDIAN(data_jobs_salary[salary_year_avg])
and
median_salary_us:=CALCULATE([median salary], data_jobs_salary[job_country]="United States")
and
median salary non us:=CALCULATE([median salary], data_jobs_salary[job_country]<>"United States")
```

Then I inserted the value functions in the field list along witht the job title:

<img width="341" height="742" alt="image" src="https://github.com/user-attachments/assets/3b4ef5d1-e687-424a-86a3-623a4ac1ccb1" />

- Interpretation: This section mainly focuses on median salary within any speceified country mainly for comparission and location decisions to make. This shows that in general a business analyst might expect to make $85,000 but if you live in the US you could expect a slight increase to $90.000 and in canada a somewhat significant decrease to $75,000 per year. 

#### Skills Likliehood
- This section further explores the skills with custom function to find what percentage is a skill likely to appear on a given job title. It can also be broken down even further by the desired country.

![EXCEL_q4W9R62Qed](https://github.com/user-attachments/assets/123468e8-d5d0-4b21-b3c1-861c5858cca0)

The formula entered in to find the skill liklihood revolves around dividing the skill count total by the job count total to get a good picture of what skills are most likely given a job title.
```
skill likliehood:=DIVIDE([skill count], [job count])
```
- Interpretation: This section gives great insight to the probability of a skill occuring within a certain job title. We can see that skills like SQL and python have high degree of probability of showing up on any job description and allows the aspiring data analyst to make intelligent decisions on what skills to learn first. 

## Conclusion
### Skills Learned
- Multi Step and Advanced Excel Formulas
- Pivot Tables
- Power Pivot
- Power Query
### The Future
- I hope to use these learned skills to advance my personal career and use it to analyze a wide variety of disciplines ready to be looked at. I think using this tool is great for piecing together massive ammounts of data and visualizing it all in one. I want to eventually use it for my passions in nuclear energy and pharmacy and uncover hidden data to help me make well informed decisions based on critical data
### Sources
- Luke Barrouse
- Jobs Data Information
