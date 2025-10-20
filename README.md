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
