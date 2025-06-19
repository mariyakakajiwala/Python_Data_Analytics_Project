# Overview

Welcome to my analysis of the data job market, focusing on data analyst roles. This project was created out of a desire to navigate and understand the job market more effectively. It delves into the top-paying and in-demand skills to help find optimal job opportunities for data analysts.

The data sourced from [Luke Barousse's Python Course](https://lukebarousse.com/python) which provides a foundation for my analysis, containing detailed information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explore key questions such as the most demanded skills, salary trends, and the intersection of demand and salary in data analytics.

# The Questions

Below are the questions I want to answer in my project:

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. What are the optimal skills for data analysts to learn? (High Demand AND High Paying) 

# Tools I Used

For my deep dive into the data analyst job market, I harnessed the power of several key tools:

- **Python:** The backbone of my analysis, allowing me to analyze the data and find critical insights.I also used the following Python libraries:
    - **Pandas Library:** This was used to analyze the data. 
    - **Matplotlib Library:** I visualized the data.
    - **Seaborn Library:** Helped me create more advanced visuals. 
- **Jupyter Notebooks:** The tool I used to run my Python scripts which let me easily include my notes and analysis.
- **Visual Studio Code:** My go-to for executing my Python scripts.
- **Git & GitHub:** Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.

# Data Preparation and Cleanup

This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.

## Import & Clean Up Data

I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.

```python
# Importing Libraries
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt  

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```

## Filter US Jobs

To focus my analysis on the UAE job market, I apply filters to the dataset, narrowing down to roles based in the United Arab Emirates only.

```python
df_US = df[df['job_country'] == 'United Arab Emirates']

```

# The Analysis

Each Jupyter notebook for this project aimed at investigating specific aspects of the data job market. Here’s how I approached each question:


## 1. What are the most demanded skills for the top 3 most popular data roles?

To find the most demanded skills for the top 3 most popular data roles, I filtered out those positions by which ones were the most popular, and got the top 5 skills for yhese top 3 roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on my target role.

View my notebook with detailed steps here: [2_Skills_Demand.ipynb](3_Project\2_Skills_Demand.ipynb)

### Visualize Data

```pyhton
fig, ax = plt.subplots(len(job_titles), 1)

for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')

plt.show()
```

### Results
![Visualization of Top Skills](3_Project\Images\skill_demand_all_data_roles.png)
*Bar chart visualizing the likelihood of skills appearing in the UAE job postings in 2023.*

### Insights

- SQL is a versatile skill, highly demanded across all three roles, but most prominently for Data Engineers (49%).
- Python is the most requested skill for Data Scientists, appearing in 46% of job postings.
- Data Engineers require more specialized technical skills (Spark, Azure, AWS) compared to Data Analysts and Data Scientists who are expected to be proficient in more general data management and analysis tools (Excel, Tableau).

## 2. How are in-demand skills trending for Data Analysts?

To find how skills are trending in 2023 for Data Analysts, I filtered data analyst positions and grouped the skills by the month of the job postings. This got me the top 5 skills of data analysts by month, showing how popular skills were throughout 2023.

View my notebook with detailed steps here: [3_Skills_Trend](3_Project\3_Skills_Trend.ipynb).

### Visualize Data

```python
df_plot = df_DA_UAE_percent.iloc[:, :5]

sns.lineplot(data=df_plot, dashes=False, palette='tab10')
sns.set_theme(style='ticks')

from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()
```
### Results

![Trending Top Skills for Data Analysts in the UAE](3_Project\Images\skill_trend_DA.png)
*Line chart visualizing the trending top skills for data analysts in the UAE in 2023.*

### Insights 
- SQL remains the most demanded skill throughout the year, although it shows a steep decrease in demand from May to July most likely due to the decrease in job postings in the UAE during that time of the year.
- Power BI experienced a significant increase in demand starting aroung April, followed by a gradual decrease around September, ultimately becoming the least demanded skill by the end of the year.
- Excel’s demand was strong and steady for most of the year, peaking in May before declining towards December. Both Python and Tableau show a relatively gradual decrease in demand throughout the year with some fluctuations.

## 3. How well do jobs and skills pay for Data Analysts?

To identify the highest-paying roles and skills, I filtered for jobs in the UAE and looked at their median salary. But first, I looked at the salary distributions of common data jobs like Data Scientist, Data Engineer, and Data Analyst, to get an idea of which jobs were paid the most. 

View my notebook with detailed steps here: [4_Salary_Analysis](3_Project\4_Salary_Analysis.ipynb).

### Visualize Data
```python
sns.boxplot(data=df_UAE_top_6, x='salary_year_avg', y='job_title_short', order=job_order)
ax = plt.gca()
ax.xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))
plt.show()
```

### Results

![Salary Distributions of Data Jobs in the UAE]
*Box plot visualizing the salary distributions for the top 6 data job titles.*

### Insights
- Data Engineers show the most variation in salaries, with a wide interquartile range and a high-end outlier around $250K, suggesting the role spans from mid-level to potentially senior or specialized positions with very high pay.
- Senior Data Analysts earn consistently higher than Data Analysts, with their salary boxes not overlapping—indicating a clear career and pay progression between the two roles.
- The Software Engineer role shows a single reported salary around $100K, which falls within the typical mid-to-upper range seen across other tech roles in the UAE, suggesting competitive compensation even with limited data. 

### Highest Paid and Most Demanded Skills for Data Analysts

Next, I narrowed my analysis and focused only on data analyst roles. I looked at the highest-paid skills and the most in-demand skills. I used two bar charts to showcase these.

### Visualize Data
```python
# Highest Paid Skills for Data Analysts
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, ax=ax[0], hue='median', palette='dark:b_r') 

# Most In-Demand Skills for Data Analysts
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, ax=ax[1], hue='median', palette='light:b') 
```

### Results
![Highest Paid and Most In-Demand Skills for Data Analysts in the UAE](3_Project\Images\highes_paid_and_most_demanded_skills_DA.png)
*Two separate bar graphs visualizing the highest-paid and most in-demand skills for data analysts in the UAE.*

### Insights

- SQL leads both in demand and pay, making it the most essential and valuable skill for Data Analysts in the current market.
- Looker ranks among the highest-paying skills but is less commonly required, suggesting it's a niche tool with premium compensation for those who know it.
- Excel and PowerPoint are highly in-demand but among the lowest-paying skills, indicating they are considered baseline or entry-level competencies.

## 4. What is the most optimal skill to learn for Data Analysis?

To identify the most optimal skills to learn (the ones that are the highest paid and highest in demand), I calculated the percent of skill demand and the median salary of these skills. To easily identify which are the most optimal skills to learn. 

View my notebook with detailed steps here: [5_Optimal_Skills](3_Project\5_Optimal_Skills.ipynb).

### Visualize Data
```python
from adjustText import adjust_text

sns.scatterplot(
    data=df_plot,
    x='skill_percent',
    y='median_salary',
    hue='technology'
)
plt.show()
```

### Visualizing Different Techonologies

Let's visualize the different technologies as well in the graph. We'll add color labels based on the technology (e.g., {Programming: Python})

### Visualize Data

```python
from matplotlib.ticker import PercentFormatter

# Create a scatter plot
scatter = sns.scatterplot(
    data=df_plot,
    x='skill_percent',
    y='median_salary',
    hue='technology'
)
plt.show()

```
### Results

![Most Optimal Skills for Data Analysts in the US with Coloring by Technology](3_Project\Images\optimal_skills.png)  
*A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the UAE with color labels for technology.*

### Insights
- SQL (programming) stands out as both the most in-demand skill (over 20% of postings) and among the highest paying (~$98K median salary), highlighting its critical role as a core technical requirement for Data Analysts in the UAE job market.
- Looker (analyst_tools) offers the highest median salary (just under $100K) but appears in fewer job postings (~5%), suggesting it’s a specialized tool with high pay and low supply—ideal for analysts aiming to stand out in niche BI platforms.
- Excel and PowerPoint (analyst_tools), along with AWS (cloud), are low-paying and low-demand skills, with salaries below $75K. These tools are likely seen as baseline or complementary, more common in entry-level or generalist roles rather than high-value technical positions.


# What I Learned

Throughout this project, I deepened my understanding of the data analyst job market and enhanced my technical skills in Python, especially in data manipulation and visualization. Here are a few specific things I learned:

- **Advanced Python Usage**: Utilizing libraries such as Pandas for data manipulation, Seaborn and Matplotlib for data visualization, and other libraries helped me perform complex data analysis tasks more efficiently.
- **Data Cleaning Importance**: I learned that thorough data cleaning and preparation are crucial before any analysis can be conducted, ensuring the accuracy of insights derived from the data.
- **Strategic Skill Analysis**: The project emphasized the importance of aligning one's skills with market demand. Understanding the relationship between skill demand, salary, and job availability allows for more strategic career planning in the tech industry.


# Insights

This project provided several general insights into the data job market for analysts:

- **Skill Demand and Salary Correlation**: There is a clear correlation between the demand for specific skills and the salaries these skills command. Advanced and specialized skills like Python and Oracle often lead to higher salaries.
- **Market Trends**: There are changing trends in skill demand, highlighting the dynamic nature of the data job market. Keeping up with these trends is essential for career growth in data analytics.
- **Economic Value of Skills**: Understanding which skills are both in-demand and well-compensated can guide data analysts in prioritizing learning to maximize their economic returns.


# Challenges I Faced

This project was not without its challenges, but it provided good learning opportunities:

- **Data Inconsistencies**: Handling missing or inconsistent data entries required careful consideration and thorough data-cleaning techniques to ensure the integrity of the analysis.
- **Complex Data Visualization**: Designing effective visual representations of complex datasets was quite challenging but critical for conveying insights clearly and compellingly.
- **Balancing Breadth and Depth**: Deciding how deeply to dive into each analysis while maintaining a broad overview of the data landscape required constant balancing to ensure comprehensive coverage without getting lost in details.


# Conclusion

This exploration into the data analyst job market was incredibly informative, highlighting the critical skills and trends that shape this evolving field. The insights I gained enhanced my understanding and also provided actionable guidance which helped advance my career in data analytics. As the market continues to change, ongoing analysis will be essential to stay ahead in data analytics. This project wass a good foundation for future explorations and underscored the importance of continuous learning and adaptation in the data field.

[def]: 3_Project\Images\salary_distributions.png