# Project - Python (Job Data Analysis)
## Abstract
A multi-library Python project that draws useful insights from existing job data. It provides useful exploratory insight about job postings in the Data sector, such as the distribution of remote/non-remote jobs, jobs with/without healthcare, top paying and most demanded data roles, and more. It further identifies the most demanded and highest paying skills for the top three data roles; it also showcases their yearly trend in job postings. After analyzing the median salaries of various Data positions, and that of the Data Scientist in particular, it unearths the most optimal skills to learn for data roles, i.e the most demanded and highest paying ones. The results are satisfying. 

**Key skills**: _Python, Pandas, MatplotLib, Seaborn, Conda_, etc. 

## Introduction

Having at least a general idea of what the job market asks for when data roles are in question would be of great use to people aiming for those kinds of positions. Luckily, it turns out there exists an abundance of high-quality data regarding job postings from all around the world. This data is made available by the well-known [Luke Barrouse](https://www.lukebarousse.com/) through his [DataNerd](https://datanerd.tech/) web-application. The database contains job postings information such as: job title, location, work type (remote or on-site), average yearly or hourly salary, salary rate, health insurance provision, skills associated with a particular job posting, company name, and much more. This data pertains to the year 2023.

The goal of the project is to analyze the data so as to provide useful insights into the job postings in general, and particularly for the Data Scientist role. The structure of the project is as follows:

1. **Exploratory Data Analysis**: What are the top data roles in the whole industry? What are the top ten countries as regards the number of job offers? What is the percentage of remote jobs, as well as jobs that provide healthcare? What are the top ten data roles in Croatia? Finally, what are the top ten most demanded data skills, in general?
2. **Skills Demand**: What are the most demanded skills for the top three most popular data roles and how do they compare quantitatively?
3. **Skills Trend**: How do the top five Data Science skills trend over the yearly cycle?
4. **Salary Analysis**: How do the median yearly salaries compare for the top six data roles? What are a) the most popular and b) the most demanded skills for the Data Science role, specifically?
5. **Optimal Skills**: What are the most optimal skills for the Data Scientist role? That is, which skills are in high demand, but are also well paid?

*Made upon the completion of Luke Barrouse's [Python course](https://www.lukebarousse.com/python). The project is helped by his material, but is not a direct or identical copy. Additionally, all data used is provided as part of his free course material.*

## Tools, files and methods 

The **tools** used in this project include:

- **Python (Anaconda distribution)**: The well-known programming language well suited for data analysis. The Anaconda distribution comes with all of the neccessary libraries, listed below.

- **Pandas**: The go-to Python library for data loading, cleaning and manipulation.

- **MatplotLib**: An excellent library used to visualize various types of data.

- **Seaborn**: The more-visually-refined younger brother of MatplotLib. Allows for more versatile graph customization, as well as color coding.

- **Seaborn**: The more-visually-refined younger brother of MatplotLib. Allows for more versatile graph customization, as well as color coding.

- **Other libraries**: _Datasets_ library for loading data from an online server, _adjustText_ library for advanced text localisation (used on scatterplots), _ast_ library for more advanced string-to-list conversion.

- **Visual Studio Code (VSCode)**: The most popular code editor; used for writing and executing Python code.

- **Jupyter Notebook**: The interactive code-development environment of choice. Allows for greater work-ethic flexibility, as well as writing comments in the Markdown format.

- **GitHub & Git**: The standard for version control, project collaboration and tracking.


The **files** uploaded to GitHub associated with this project include: 

- [**Python code**](/): five distinct .ipynb files that provide answers to the questions laid out in the Introduction.

- [**Visualizations (.png)**](/images/): all project data visualizations stored in one place.

The **methodology** of the whole project is quite straight-forward: 
- Use VSCode as the code editor of choice, and Jupyter Notebook as our working environment.
- Use Python (Pandas) to import, clean and manipulate the data in order to find answers to specified questions.
- Use MatplotLib and Seaborn to visualize our findings.
- Additionally, use other miscellaneous libraries when necessary.

## Results and analysis
Following the methodology laid out in the previous section, an effort was made to answer the questions specified in the introductory section.

### 1. Exploratory Data Analysis
Every data analysis task starts with getting a general idea of the dataset we're working with. With that in mind we firstly import all necessary libraries and load the dataset using the ```load_dataset``` function from the _datasets_ library. After converting the datasets to a Pandas DataFrame object, we convert the ```job_posted_date``` string column to a proper ```datetime``` object. Additionally, the ```job_skills``` column, containing a list of corresponding job skills _in string format_ is converted to a real list-object using the ```.literal_eval``` method from the _ast_ library.

```python
#Importing libraries
import pandas as pd
import matplotlib.pyplot as plt
from datasets import load_dataset
import ast

#Importing data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

#Cleaning data
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda skills: ast.literal_eval(skills) if pd.notna(skills) else skills)
```
After cleaning and importing our data, we move on to identify the top 10 most popular job positions in the data industry. For, this, we count the total number of a specific job posting using the .```value_counts()``` methd and divide it be the total number of job postings in the dataset using the ```.count()``` method. We then plot the results the 'Pandas' way', which is really MatplotLib in disguise.
```python
total_data_jobs = df['job_title_short'].count()
(df['job_title_short'].value_counts().head(10)/total_data_jobs).plot(kind='barh', color="blue")

plt.title("Top 10 Data Roles (all countries)")
plt.xlabel("Ratio")
plt.ylabel("")

ax = plt.gca()
ax.invert_yaxis()

plt.savefig('images/01_Top_10_Data_Roles.png', bbox_inches='tight')
plt.show()
```
![01_Top_10_Data_Roles](/images/01_Top_10_Data_Roles.png)

**Image 1.** _The top 10 data roles (all countries)._

We then look at the top ten data roles in Croatia. This we do by filtering for Croatia-based jobs only. 

```python
df_croatia = df[df['job_country']=="Croatia"].copy()
```
What follows is manipulating the tables and plotting data using the same code as before, giving us the result shown in Image 2.

![01_Top_10_Data_Roles_Croatia](/images/01_Top_10_Data_Roles_Croatia.png)

**Image 2.** _The top 10 data roles (Croatia)._

To find the top ten most popular skills in the whole data industry, we reuse the same code, making sure that the ```job_skills``` column, containing a _list_ of necessary skills, is 'unpacked' through the use of the ```.explode()``` method.
```python
df_skills = df.explode('job_skills')
total_skills_number = df_skills['job_skills'].count()
(df_skills['job_skills'].value_counts()/total_skills_number).head(10).plot(kind='barh', color="green")
plt.title("Most Demanded Data Skills")
plt.xlabel("Ratio")
plt.ylabel('')
ax = plt.gca()
ax.invert_yaxis()

plt.savefig('images/01_Most_Demanded_Data_Skills_Overall.png', bbox_inches='tight')
plt.show()
```

![01_Most_Demanded_Data_Skills_Overall](/images/01_Most_Demanded_Data_Skills_Overall.png)

**Image 3.** _The top 10 most demanded data skills._

In a similar manner, we obtain the top countries in terms of job offerings.

![01_Top_10_Countries](/images/01_Top_10_Countries.png)

**Image 4.** _The top 10 countries by count job offerings._

Following the same methods, but using bar-plots, we find the percentages of remote/non-remote jobs, as well as jobs that provide healthcare.

![01_Remote_jobs_pie](/images/01_Remote_jobs_pie.png)

**Image 5.** _The percentage of remote vs. non-remote jobs._

![01_Health_insurance_jobs_pie](/images/01_Health_insurance_jobs_pie.png)

**Image 6.** _The percentage of healthcare-providing jobs._

As expected, the top data roles are Data Analyst, Scientist and Engineer, respectively. SQL and Python boldly dominate as the most sought-after data skills by a large margin. The countries in which IT is largely developed tend to offer the most jobs, and they include the US, India, the UK, France and Germany. It is interesting that in Croatia there is a higher demand for Data Engineers. It's also surprising that even in 2023 the percentage of remote jobs is only around 9 percent. Data Jobs that ensure Healthcare are likewise in the small minority.

### 2. Skill Demand
After finding out that the top three most popular roles in the data industry are Data Analyst, Data Scientist, and Data Engineer, respectively, we look at each one individually and look for the top five skills. Similar to the previous section, we prepare the data for visualization using common Pandas methods. Since we wish to show the percentage among all job postings, we make sure to divide the count of individual skills by the total number of job postings (a number). The following code uses the Data Analyst role as an example.

```python
df_skills_analyst_orig = df[df['job_title_short'] == "Data Analyst"]
df_skills_analyst = df_skills_analyst_orig.explode('job_skills')
total_skills_analyst = df_skills_analyst_orig['job_title_short'].size
(df_skills_analyst['job_skills'].value_counts()/total_skills_analyst).head(10).plot(kind='barh', color="green")
plt.title("Most Demanded Skills - Data Analyst")
plt.xlabel("Ratio")
plt.ylabel('')
ax = plt.gca()
ax.invert_yaxis()
```
After doing the same for the other two roles, we plot them all on one canvas using the _Seaborn_ library.

```python
df_skills_analyst = (df_skills_analyst['job_skills'].value_counts()/total_skills_analyst*100).head(5).reset_index(name="percentage").copy()
df_skills_engineer = (df_skills_engineer['job_skills'].value_counts()/total_skills_engineer*100).head(5).reset_index(name="percentage").copy()
df_skills_scientist = (df_skills_scientist['job_skills'].value_counts()/total_skills_scientist*100).head(5).reset_index(name="percentage").copy()

fig, ax = plt.subplots(3, 1)  
sns.set_theme(style='ticks')

sns.barplot(data=df_skills_analyst, x='percentage', y='job_skills', ax=ax[0], hue='job_skills', palette='Greens_r') 
sns.barplot(data=df_skills_engineer, x='percentage', y='job_skills', ax=ax[1], hue='job_skills', palette="Blues_r")
sns.barplot(data=df_skills_scientist, x='percentage', y='job_skills', ax=ax[2], hue='job_skills', palette='Purples_r')

ax[0].set_title('Data Analysts')
ax[0].set_ylabel('')
ax[0].set_xlabel('')
ax[0].set_xlim(0,70)
ax[0].set_xticks([])

ax[1].set_title('Data Enginners')
ax[1].set_ylabel('')
ax[1].set_xlabel('')
ax[1].set_xlim(0,70)
ax[1].set_xticks([])

ax[2].set_title('Data Scientists')
ax[2].set_ylabel('')
ax[2].set_xlabel('Probability of Skill Being in Job Posting')
ax[2].set_xlim(0,70)
ax[2].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'{x:.0f}%'))


fig.suptitle("Most Demanded Skills for Top 3 Data Roles")
fig.tight_layout()
plt.savefig('images/02_Most_Demanded_Skills_per_role.png', bbox_inches='tight')
plt.show()
```

Here we make use of the Seaborn ```.barplot()``` method and also utilize the ```hue='job_skills'``` parameter to grade the colors accordingly. The rest is chart formatting.

![02_Most_Demanded_Skills_per_rolel](/images/02_Most_Demanded_Skills_per_role.png)

**Image 7.** _The top 5 most demanded data skills for the top 3 data roles._

It's important to note that SQL and Python tend to dominate across all of the three categories. Analysts will do good to learn Excel and data visualization software such as Tableau and Power BI; Engineers can focus on cloud technologies such as AWS or Azure; Scientists should get to know statistical analysis languages and software such as R and SAS.

### 3. Data Science Yearly Skill Trend
Now, we shift our attention to the Data Science role only. We wish to determine how the skill demand for this role varies throughout the yearly cycle. We start by preparing the data. First, we filter only for Data Scientist roles, generate a new column containing the month of the ```job_posted_date``` (for this we use the ```.dt.month ```method) and 'explode' the data based on the ```job_skills``` column. 
```python
df_scientist = df[df['job_title_short'] == 'Data Scientist'].copy()
df_scientist ['job_posted_month'] = df_scientist['job_posted_date'].dt.month
df_scientist_explode = df_scientist.explode('job_skills')
```
We then go on to make a pivot table based on the ```job_posted_month``` and ```job_skills``` columns, using ```agg='size'``` as the aggregation parameter. Additionally, we create a new ```'Total'``` row which serves as a sum aggregation of the values above.

```python
df_pivot1 = df_scientist_explode.pivot_table(index='job_posted_month', columns='job_skills', aggfunc='size', fill_value=0)
df_pivot1.loc['Total'] = df_pivot1.sum()
```
Displaying the first ten columns of ```df_pivot1``` provides results as shown in Table 1. 
| job_posted_month   |   airflow |   airtable |   alteryx |   angular |   angular.js |   ansible |   apl |   arch |   asana |   asp.net |
|:-------------------|----------:|-----------:|----------:|----------:|-------------:|----------:|------:|-------:|--------:|----------:|
| 1                  |       494 |          5 |       301 |       109 |            1 |        58 |     6 |     16 |      11 |        19 |
| 2                  |       331 |          4 |       199 |        59 |            1 |        38 |     6 |      7 |       1 |        21 |
| 3                  |       322 |          1 |       165 |        61 |            0 |        36 |     8 |      7 |       4 |        12 |
| 4                  |       338 |          5 |       147 |        49 |            2 |        40 |    13 |      5 |       4 |        13 |
| 5                  |       276 |          8 |       144 |        51 |            2 |        37 |     3 |      8 |       4 |        11 |
| 6                  |       266 |          9 |       157 |        79 |            0 |        30 |     8 |      7 |      11 |        16 |
| 7                  |       300 |          6 |       187 |        58 |            0 |        20 |    15 |      1 |      20 |         8 |
| 8                  |       374 |          4 |       213 |        72 |            0 |        31 |    13 |      6 |      11 |        15 |
| 9                  |       315 |          7 |       156 |        58 |            2 |        41 |     9 |      6 |       8 |         8 |
| 10                 |       346 |          7 |       137 |        50 |            2 |        47 |     7 |      2 |       7 |         8 |
| 11                 |       332 |          6 |       144 |        56 |            2 |        31 |     5 |      3 |       9 |         5 |
| 12                 |       221 |          3 |       118 |        34 |            0 |        39 |     8 |      7 |       8 |         4 |
| Total              |      3915 |         65 |      2068 |       736 |           12 |       448 |   101 |     75 |      98 |       140 |

**Table 1.** _Data Science skills pivot table._

We then sort by the Total row and then delete it, because it no longer serves any purpose.
```python
#Sorting by 'Total' row and dropping that row afterwards
df_pivot1 = df_pivot1[df_pivot1.loc['Total'].sort_values(ascending=False).index]
df_pivot1 = df_pivot1.drop('Total')
```
In the hopes of calculating percentages of skills in job postings for each month, we first determine the total number of job postings in each month.

```python
df_scientist_total_jobs_month = df_scientist.groupby('job_posted_month').size()
```

The last step before plotting the data is to divide the skill counts by the total number of job postings for each month. To do this, we use the Pandas ```.div()``` method. Afterwards, we reindex the DataFrame by the ```job_posted_month``` column translated into a string format using the ```.apply()``` method and ```lambda``` functions.

```python
#expressing the skill-demand trend in terms of percentages
df_pivot1_percent = df_pivot1.div(df_scientist_total_jobs_month/100, axis=0)

#reindexing the month to be in string format
df_pivot1_percent = df_pivot1_percent.reset_index()
df_pivot1_percent['job_posted_month'] = df_pivot1_percent['job_posted_month'].apply(lambda x: pd.to_datetime(x, format='%m').strftime('%b'))
df_pivot1_percent = df_pivot1_percent.set_index("job_posted_month")
```
Now we plot the results using the Seaborn library and format the charts.
```python
#df_pivot1_percent.iloc[:,:5].plot(kind='line')
df_plot = df_pivot1_percent.iloc[:,:5]
sns.lineplot(data=df_plot, dashes=False, palette='tab10')
sns.set_theme(style='ticks')
sns.despine()

plt.xlabel('')
plt.ylabel('Percentage')
ax=plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'{int(y)}%'))

plt.legend(loc="lower right").remove()
plt.title("Demand of Top 5 Data Science Skills over the Year")

for i in range(5):
    plt.text(11.7, df_plot.iloc[-4, i], df_plot.columns[i])

plt.savefig('images/03_Skills_trend.png', bbox_inches='tight')
plt.show()
```

![03_Skills_Trend](/images/03_Skills_trend.png)

**Image 8.** _The yearly trend of the top 5 most demanded Data Science skills._

We find that all of the skills tend to stay relatively stable throughout the whole yearly cycle. There is a slight dip in demand in the autumn months which is followed by a general resurgence at the beginning of the year when employers are actively hiring.
### 4. Salary Analysis 
In this section, we want to compare the median salaries for the top six data roles and visualize the findings using boxplots. We start by removing all rows that have ```NULL``` values in the ```salary_year_avg``` column. This ensures accurate data visualization later on. Moving forward, we identify the top six most popular data roles and store them into a list using the ```.value_counts()```, ```.index```, and ```.to_list()``` methods, respectively. We then filter our DataFrame for these six roles only, group based on the job_title_short and salary_year_avg columns, perform a 'median'-type aggregation and sort the values in descending order. The indices of the resultant DataFrame are put into a list called ```jobs_sorted```. This list is used when plotting to ensure a right ordering of the medians for different roles. Specifically, it is done through the ```order=jobs_sorted``` parameter in the Seaborn ```.boxplot()``` method.

```python
df = df.dropna(subset=['salary_year_avg'])
top_6_jobs = df['job_title_short'].value_counts().index[:6].to_list()
df_top_6 = df[df['job_title_short'].isin(top_6_jobs)]
jobs_sorted = df_top_6.groupby('job_title_short')['salary_year_avg'].agg('median').sort_values(ascending=False).index.to_list()

sns.boxplot(data=df_top_6, x='salary_year_avg', y='job_title_short', order=jobs_sorted)
sns.set_theme(style='ticks')

plt.title("Yearly Salaries of Top 6 Data Roles")
plt.xlabel('Yearly Salary (USD)')
plt.ylabel('')

plt.gca().xaxis.set_major_formatter(lambda x, pos: f'${int(x/1000)}K')
plt.xlim(0,600e3)
plt.savefig('images/04_Salary_Top_6_Data_roles.png', bbox_inches='tight')
plt.show()
```
![04_Salary_Top_6_Data_roles](/images/04_Salary_Top_6_Data_roles.png)

**Image 9.** _Yearly salaries of top 6 data roles._

The second portion of this section is to concentrate on Data Science skills and find those that are most popular (we have already done that in Section 2., but now we expand their number) and associated with the highest pay. The process used to arrive at these findings is similar to the previous instances of data manipulation and will not be repeated here.

![04_Most_Popular_and_Highest_Paying_Skills](/images/04_Most_Popular_and_Highest_Paying_Skills.png)

**Image 10.** _Most popular and highest paying Data Science skills._

A surprising discovery is that Senior Data Analyst roles have a lower median yearly salary than that of junior Data Scientists or Engineers. The last chart shows us that the highest paying Data Science skills tend to be the most niche ones, i.e. skills that require specialization (e.g. Asana, AirTable, Watson). We can safely conclude that these are associated with well established and higher ranked roles.

### 5. Most Optimal Data Science Skills
In the last section we concentrate on finding the most optimal Data Science skills overall. This means identifying those that are both popular and well-paid. The process of finding the skills is relatively straight-forward and similar to our previous queries. Here we will focus on the skill categorisation part of the problem. The original DataFrame contains a job_type_skills column which inludes a string-type dictionary linking each skill to a specific data technology sector. This information is used to color-code our final scatter plot, as shown in the code below.

```python
df_technology = df['job_type_skills'].copy()

# remove duplicates
df_technology = df_technology.drop_duplicates()

# remove NaN values
df_technology = df_technology.dropna()

# combine all dictionaries into one
technology_dict = {}
for row in df_technology:
    row_dict = ast.literal_eval(row)  # convert string to dictionary
    for key, value in row_dict.items():
        if key in technology_dict:  # if key already exists in technology_dict, add value to existing value
            technology_dict[key] += value
        else:                       # if key does not exist in technology_dict, add key and value
            technology_dict[key] = value

# remove duplicates by converting values to set then back to list
for key, value in technology_dict.items():
    technology_dict[key] = list(set(value))

#convert to DataFrame
df_technology = pd.DataFrame(list(technology_dict.items()), columns=['technology','skills'])
df_technology = df_technology.explode('skills')

df_DS_skills_tech = df_DS_skills.merge(df_technology, left_on='job_skills', right_on='skills')
df_DS_skills_tech = df_DS_skills_tech.set_index('skills')
```
The code above consists of cleaning the ```job_type_skills``` column of dupicates and ```NaN``` values, combining the individual dictionaries into one large dictionary, removing its duplicates and converting it to a Pandas DataFrame.

Everything is then plotted on the final scatter plot. Additionally the, _adjustText_ library is used for ensuring that the different scatter labels (names of skills) do not overlap. When necessary, it also plots gray lines connecting the label to the point on the scatter plot. The rest of the code is more or less advanced chart formatting.

```python
sns.scatterplot(data=df_DS_skills_tech, x='skill_percentage', y='median_salary', hue='technology')
sns.despine()
sns.set_theme(style='ticks')

plt.xlabel('Percent of Data Scientist Jobs')
plt.ylabel('Median Yearly Salary')
plt.title(f'Median Salary vs. Market Share for Top {len(df_DS_skills)} DS Skills')

#Preparing texts for adjustText
texts = []
for i, txt in enumerate(df_DS_skills_tech.index):
    texts.append(plt.text(df_DS_skills_tech['skill_percentage'].iloc[i], df_DS_skills_tech['median_salary'].iloc[i], txt))

# Adjust text to avoid overlap
adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray'))

# Get current axes, set limits, and format axes
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))  # Example formatting y-axis

ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))
ax.xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'{int(x)}%'))
# Adjust layout and display the plot
plt.tight_layout()
plt.savefig('images/05_Optimal_skills.png', bbox_inches='tight')
plt.show()
```
![05_Optimal_skills](/images/05_Optimal_skills.png)

**Image 11.** _Most optimal Data Science skills; they are the ones with the largest job market share and median yearly salary. That is, the uppermost ones farthest to the right._

We see that, for the Data Scientist role, SQL, Python and R provide strong median yearly salaries while also being highly demanded by employers. Comparing Tableau and PowerBI (data visualization software), we see that the former is both better paid and more sought after. Machine Learning libraries such as PyTorch, TensorFlow, and ScikitLearn, as well as the data manipulation library Pandas are all very well paid, but seemingly reserved for more established Data Science positions.
## Conclusion
To end our analysis, we can lay out our findings in a few compact and digestible bullet-points:

- The top 3 data roles in all countries are Data Analyst, Data Engineer and Data Scientist, respectively. In Croatia, Data Engineers take the lead, outperforming Analysts.
- The countries with the highest number of job postings in the data industry include the US, India and the UK, among others. They are countries in which the IT industry is prosperous.
- Only 10% of all data jobs are remote, and only 11% provide healthcare.
- Across the top 3 data roles, Python and SQL are skills that boldly dominate with respect to their demand.
- Data Analysts would prosper by learning Tableau or Power BI, Engineers by looking at AWS or Azure, and Scientist by getting to know R and SAS.
- The yearly demand of Data Science skills is quite stable, with a slight peak in the first few months of the year. This is probably correlated to employers hiring.
- Senior Data Scientists have the highest median yearly salary, and are followed by Senior Data Engineers. Interestingly, the next in line is the junior Data Scientist and Engineer role, outperforming even the Senior Data Analysts, aside from the last, namely the junior.
- The highest paying Data Science skills are the most niche ones, i.e. those associated with specialized and well-established (Senior) Data Science roles.
- The most optimal skills to learn for Data Scientists are by a large margin SQL and Python, which are followed by R.

<br>
<br>  
<br>                 <div style="text-align: right"> ~ Gregoritsch3 </div>  
