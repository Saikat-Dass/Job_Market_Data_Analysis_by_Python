<h1>Tools I used to deep dive into the data analyst job market :</h1>

1. Python
2. Pandas Library
3. Matplotlib Library
4. Seaborn Library
5. Jupyter Notebooks
6. Visual Studio Code

<h1>Importing Libraries</h1>

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import ast
```
<h1>Cleaning up the data</h1>

```python
from datasets import load_dataset
data = pd.read_csv('https://lukeb.co/python_csv')
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else(x))
```
<h1>Below are the questions I want to answer in my project:</h1>

<h2>1. What are the skills most in demand for the top 3 most popular data roles?</h2>

```python
fig, ax = plt.subplots(len(Titles),1)
sns.set_theme(style='ticks')
for i, title in enumerate(Titles):
    df_plotting = df_skills_percent[df_skills_percent['job_title_short']== title].head(5)
    sns.barplot(data= df_plotting, x= 'Skill Percent', y='job_skills', ax= ax[i], hue='Skill Percent', palette='dark:g_r', legend= False)
    ax[i].set_title(title, fontsize= 15)
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].set_xlim(0,70)

    for p, txt in enumerate(df_plotting['Skill Percent']):
        ax[i].text(txt +1, p, f'{txt:.0f}%', va= 'center')

fig.suptitle('\u0330'.join('Percentage of skills in US Job Postings'), fontsize= 18, fontweight= 'bold')
fig.tight_layout()
plt.show()
```
![Bar graph visualizing the salary for the top 3 data roles](https://github.com/SaikatDaas/Python_Data_Analysis_Project/blob/1ea211186561a1e3475033d03e197a8508af09b6/Project%20Python/Bar%20graph%20visualizing%20the%20salary%20for%20the%20top%203%20data%20roles.png)

<h3>Insights</h3>

- SQL is the most requested skill for Data Analysts and Data Scientists, with it in over half the job postings for both roles. For Data Engineers, Python is the most sought-after skill, appearing in 68% of job postings.
- Data Engineers require more specialized technical skills (AWS, Azure, Spark) compared to Data Analysts and Data Scientists who are expected to be proficient in more general data management and analysis tools (Excel, Tableau).
- Python is a versatile skill, highly demanded across all three roles, but most prominently for Data Scientists (72%) and Data Engineers (65%).

<h2>2. How are in-demand skills trending for Data Analysts?</h2>

```python
sns.lineplot(data= df_plot, dashes= False, palette='tab10', legend= False)
sns.despine()
plt.title('\u0330'.join('Trending Top Skills for Data Analysts in the US'), fontsize = 14, fontweight = 'bold', pad= 15)
plt.ylabel('Job Postings', fontsize = 12)
plt.xlabel('2024', fontsize = 12)
plt.gca().yaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'{x:.0f}%'))
for i in range(5):
    plt.text(11.2, df_plot.iloc[-1,i], df_plot.columns[i])
plt.tight_layout()
plt.show()
```
![Line graph visualizing the trending top skills](https://github.com/SaikatDaas/Python_Data_Analysis_Project/blob/1722dc5c341bdb5701aa87564ac88c1a2f602c75/Project%20Python/Line%20graph%20visualizing%20the%20trending%20top%20skills.png)

<h3>Insights</h3>

- SQL remains the most consistently demanded skill throughout the year, although it shows a gradual decrease in demand.
- Excel experienced a significant increase in demand starting around September, surpassing both Python and Tableau by the end of the year.
- Both Python and Tableau show relatively stable demand throughout the year with some fluctuations but remain essential skills for data analysts. Power BI, while less demanded compared to the others, shows a slight upward trend towards the year's end.

<h2>3. How well do jobs and skills pay for Data Analysts?</h2>

```python
sns.boxplot(data=df_titles, x='salary_year_avg', y='job_title_short', order=df_top_titles)
sns.set_theme(style='ticks')
sns.despine()
plt.title('\u0330'.join('Salary Distributions of Data Jobs in the US'), fontsize= 16, fontweight= 'bold', pad=20)
plt.xlabel('Yearly Salary (USD)', fontsize= 12)
plt.ylabel('')
plt.xlim(0,600000) 
plt.gca().xaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))
plt.tight_layout(h_pad= 2.5)
plt.show()
```
![Box plot visualizing the salary distributions](https://github.com/SaikatDaas/Python_Data_Analysis_Project/blob/1722dc5c341bdb5701aa87564ac88c1a2f602c75/Project%20Python/Box%20plot%20visualizing%20the%20salary%20distributions.png)

<h3>Insights</h3>

- There's a significant variation in salary ranges across different job titles. Senior Data Scientist positions tend to have the highest salary potential, with up to $600K, indicating the high value placed on advanced data skills and experience in the industry.
- Senior Data Engineer and Senior Data Scientist roles show a considerable number of outliers on the higher end of the salary spectrum, suggesting that exceptional skills or circumstances can lead to high pay in these roles. In contrast, Data Analyst roles demonstrate more consistency in salary, with fewer outliers.
- The median salaries increase with the seniority and specialization of the roles. Senior roles (Senior Data Scientist, Senior Data Engineer) not only have higher median salaries but also larger differences in typical salaries, reflecting greater variance in compensation as responsibilities increase.

### Highest Paid & Most Demanded Skills for Data Analysts

```python
fig, ax = plt.subplots(2, 1)  
sns.barplot(data=df_top_pay, x='median', y=df_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r', legend= False)
sns.despine()
ax[0].set_title('\u0330'.join('Highest Paid Skills for Data Analysts in the US'), fontsize= 16, fontweight= 'bold', pad= 20)
ax[0].set_ylabel('')
ax[0].set_xlabel('Median Salary', fontsize= 14)
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))



sns.barplot(data=df_skills, x='median', y=df_skills.index, hue='median', ax=ax[1], palette='dark:r_r', legend= False)
ax[1].set_title('\u0330'.join('Most In-Demand Skills for Data Analysts in the US'), fontsize= 16, fontweight= 'bold', pad= 15)
ax[1].set_ylabel('')
ax[1].set_xlabel('Median Salary', fontsize= 14)
ax[1].set_xlim(ax[0].get_xlim())
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'${int(x/1000)}K'))

sns.set_theme(style='ticks')
plt.tight_layout(h_pad=2)
plt.show()
```
![highest paid skills and most in-demand skills](https://github.com/SaikatDaas/Python_Data_Analysis_Project/blob/1722dc5c341bdb5701aa87564ac88c1a2f602c75/Project%20Python/highest%20paid%20skills%20and%20most%20in-demand%20skills.png)

<h3>Insights</h3>

- The top graph shows specialized technical skills like dplyr, Bitbucket, and Gitlab are associated with higher salaries, some reaching up to $200K, suggesting that advanced technical proficiency can increase earning potential.
- The bottom graph highlights that foundational skills like Excel, PowerPoint, and SQL are the most in-demand, even though they may not offer the highest salaries. This demonstrates the importance of these core skills for employability in data analysis roles.
- There's a clear distinction between the skills that are highest paid and those that are most in-demand. Data analysts aiming to maximize their career potential should consider developing a diverse skill set that includes both high-paying specialized skills and widely demanded foundational skills.

<h2>4. What are the optimal skills for data analysts to learn? (High Demand AND High Paying)</h2>

```python
sns.scatterplot(data= df_DA_skills, x='skill percent', y='median salary', hue='technology')
sns.despine()
sns.set_theme(style='ticks')

texts = []
for i, txt in enumerate(df_DA_skills['job_skills']):
    texts.append(plt.text(df_DA_skills['skill percent'].iloc[i], df_DA_skills['median salary'].iloc[i], txt))

adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray'))
plt.xlabel('Percent of Data Analyst Jobs')
plt.ylabel('Median Salary')
plt.title('Most Optimal Skills for Data Analysts in the US')
plt.legend(title='Technology')
plt.gca().xaxis.set_major_formatter(plt.FuncFormatter(lambda x, pos: f'{x:.0f}%'))
plt.gca().yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}k'))
plt.tight_layout()
plt.show()
```
![A scatter plot visualizing the most optimal skills (high paying & high demand)](https://github.com/SaikatDaas/Python_Data_Analysis_Project/blob/1722dc5c341bdb5701aa87564ac88c1a2f602c75/Project%20Python/A%20scatter%20plot%20visualizing%20the%20most%20optimal%20skills%20(high%20paying%20%26%20high%20demand).png)

<h3>Insights</h3>

- The skill Oracle appears to have the highest median salary of nearly $97K, despite being less common in job postings. This suggests a high value placed on specialized database skills within the data analyst profession.
- More commonly required skills like Excel and SQL have a large presence in job listings but lower median salaries compared to specialized skills like Python and Tableau, which not only have higher salaries but are also moderately prevalent in job listings.
- Skills such as Python, Tableau, and SQL Server are towards the higher end of the salary spectrum while also being fairly common in job listings, indicating that proficiency in these tools can lead to good opportunities in data analytics.

<h1>Challenges I Faced</h1>

- Data Inconsistencies :- Handling missing or inconsistent data entries requires careful consideration and thorough data-cleaning techniques to ensure the integrity of the analysis.
- Complex Data Visualization :- Designing effective visual representations of complex datasets was challenging but critical for conveying insights clearly and compellingly.
- Balancing Breadth and Depth :- Deciding how deeply to dive into each analysis while maintaining a broad overview of the data landscape required constant balancing to ensure comprehensive coverage without getting lost in details.
