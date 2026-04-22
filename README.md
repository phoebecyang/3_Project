# Overview
Welcome to my analysis on the data job market, especially on the role of data analyst. This project was used to help people understand the data job market better and to help data analysts find optimal job opportunities.

# The Questions
These are the questions I want to answer in my project.

1. What are the skills most in demand for the top 3 most popular data roles?

2. How are in-demand skills trending for Data Analysts?

3. How well do jobs and skills pay for Data Analysts?

4. What are the optimal skills for data analysts to learn? (High Demand AND High Paying)

# Tools I used 
1. Python: Every calculation, cleaning the data and plotting was all done on Python. With the use of these libraries: Pandas, Metplotlib and Seaborn
2. Jupyter Notebook
3. Visual Studio Code
4. Git&Github
 
# Import and Clean Up the Data
I start by loading up the data cleaning up the data and import the necessary libraries.

# Filter US Jobs
Since the project will focus on job postings in the United States, I then filter the data to get the roles in the US.

# The Analysis
Each questions in this project is answer seperately in Jupyter notebook, below is how I approach each question:

# 1. What are the skills most in demand for the top 3 most popular data roles?

To answer this question, first I filtered out the positions to find the top 3 most popular roles. Then by highlighting the roles I can use the data to draw the plot. 
This is the detailed codes and steps of how I plot the chart:[2_Skill_Demand](2_Skill_Demands.ipynb)

# Visualize Data
```python
fig, ax = plt.subplots(len(job_titles),1)

sns.set_theme(style='ticks')
for i, job_title in enumerate(job_titles):
    df_plot = df_skill_perc[df_skill_perc['job_title_short'] == job_title].head(5)[::-1]
    sns.barplot(data=df_plot,x = 'skill_percent', y = 'job_skills', ax=ax[i], hue='skill_percent', palette='dark:b_r')
    ax[i].set_title(job_title)
    ax[i].invert_yaxis()
    ax[i].set_xlabel('')
    ax[i].set_ylabel('')
    ax[i].get_legend().remove()
    ax[i].set_xlim(0, 80)
     # remove the x-axis tick labels for better readability
    if i != len(job_titles) - 1:
        ax[i].set_xticks([])

    # label the percentage on the bars
    for n, v in enumerate(df_plot['skill_percent']):
        ax[i].text(v + 1, n, f'{v:.0f}%', va='center')
fig.suptitle('Percentage of Skills Requested for United States Job Postings')
fig.tight_layout(h_pad=0.8)
plt.show()
```

# Result
![Skill_demand_plot](percent_skill_demand.png)
Bar plot illustrate the top 3 roles and the top 5 skills related to each one

# Insights
1. From the plot, we can see SQL is the most requested skill for all three positions, with it being required for about half of all the positions.
2. Data Engineer requires more specialized skills(aws, azure, spark) compare to data analyst and data scientist who are expected to be more proficient in more generalized data management and analysis tools(excel, sql)
3. Python is a versatile skill, highly demanded across all three roles, but most prominently for Data Scientists (72%) and Data Engineers (65%).

# 2. How are in-demand skills trending for Data Analysts?
To find out the in-demand skills of the job postings in 2023, I filtered out data analysts positions and grouped the skills by month. 

# Visulaize data

```python
from matplotlib.ticker import PercentFormatter

df_plot = df_DA_US_percent.iloc[:,:5]
sns.lineplot(data=df_plot, dashes=False, legend='full',palette='tab10')
sns.set_theme(style='ticks')
sns.despine()

plt.title('Trending Top Skills for Data Analysts in the US (Percentage)')
plt.ylabel('Percentage of Job Postings')
plt.xlabel('2023')
plt.legend().remove()
plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

for i in range(5):
    plt.text(12, df_plot.iloc[-1, i], df_plot.columns[i], color='black')
```
![Skill_Trend_Plot](skill_trend_percent.png)

# Insights
1. SQL remains the most requested skill throughout the year though it shows a slow decrease.
2. Excel remains steady throughout the year though it slightly decrease in August and September, and Excel also surpass Python and Tableau throughout the entire year.
3. SQL, Excel, and Python are the top 3 most requested skills among all the job postings. With Python surpassing Tableau in November and shows an increasing trend after.

# 3. How well do jobs and skills pay for Data Analysts?
To identify the highest paid jobs and skills, I only look for the data in United States. But I first look at the salary of common data jobs to see which job gets paid the most.

The complete code: [4_Jobskill_Pay](4_Jobskill_Pay.ipynb)

# Visual Data
```python
sns.boxplot(data=df_us_top6, x= 'salary_year_avg', y = 'job_title_short', order=job_order)
sns.set_theme(style='ticks')
sns.despine()

plt.title('Salary Distribution of Top 6 Job Titles in the US')
plt.xlabel('Average Yearly Salary (USD)')
plt.ylabel('')
plt.xlim(0, 600000)
ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```

# Result

![highest_paid_job](salary_distribution.png)

# Insight
1. There's a significant variation in salary ranges across different job titles. Senior Data Scientist positions tend to have the highest salary potential, with up to $600K, indicating the high value placed on advanced data skills and experience in the industry.

2. Senior Data Engineer and Senior Data Scientist roles show a considerable number of outliers on the higher end of the salary spectrum, suggesting that exceptional skills or circumstances can lead to high pay in these roles. In contrast, Data Analyst roles demonstrate more consistency in salary, with fewer outliers.

3. The median salaries increase with the seniority and specialization of the roles. Senior roles (Senior Data Scientist, Senior Data Engineer) not only have higher median salaries but also larger differences in typical salaries, reflecting greater variance in compensation as responsibilities increase.

# Highest Paid & Most Demanded Skills for Data Analysts
Next, I narrowed my analysis and focused only on data analyst roles. I looked at the highest-paid skills and the most in-demand skills. I used two bar charts to showcase these.

# Visualize Data

```python
fig, ax = plt.subplots(2, 1)  

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_da_top_pay, x='median', y=df_da_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')
ax[0].legend().remove()
# original code:
# df_DA_top_pay[::-1].plot(kind='barh', y='median', ax=ax[0], legend=False) 
ax[0].set_title('Highest Paid Skills for Data Analysts in the US')
ax[0].set_ylabel('')
ax[0].set_xlabel('')
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))


# Top 10 Most In-Demand Skills for Data Analysts')
sns.barplot(data=df_da_skills, x='median', y=df_da_skills.index, hue='median', ax=ax[1], palette='light:b')
ax[1].legend().remove()
# original code:
# df_DA_skills[::-1].plot(kind='barh', y='median', ax=ax[1], legend=False)
ax[1].set_title('Most In-Demand Skills for Data Analysts in the US')
ax[1].set_ylabel('')
ax[1].set_xlabel('Median Salary (USD)')
ax[1].set_xlim(ax[0].get_xlim())  # Set the same x-axis limits as the first plot
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))

sns.set_theme(style='ticks')
plt.tight_layout()
plt.show()
```

# Result
![highest_paid_skills](highest_paid_skill.png)

# Insight
1. The top graph shows dplyr, bitbucket and gitlab as the highest paid skills for Data Analysts in the US. With yearly median salary being up to nearly 200k for dpylr. This suggests advance technical skills may result in a higher income.

2. The bottom graph highlights that foundational skills like Excel, PowerPoint, and SQL are the most in-demand, even though they may not offer the highest salaries. This demonstrates the importance of these core skills for employability in data analysis roles.

3. There is a clear difference between the highest-paid and the most in-demand skills. For data analysts who want to reach the maxium potential of their careers can considering developing diverse skills that include both high-paying specialized skill and popular in-demand skills.