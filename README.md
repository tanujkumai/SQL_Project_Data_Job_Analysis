# 📊 Data Analyst Job Market Exploration (SQL Project)

Hey there! 👋  
This project is part of my journey into data analytics. I used SQL to explore trends in the data analyst job market — including the top-paying jobs, in-demand skills, and the best tools to learn for landing strong opportunities.

🧠 **Goal:** Practice SQL and gain insights into what skills matter in real-world job postings.

🔗 **SQL Queries?** You can find them in the [`/project_sql`](./project_sql/) folder.

> ⚠️ I’m still learning — this is a personal practice project, not expert advice. Feedback is always welcome!

---

## 📌 What This Project Is About

I used a dataset from [Luke Barousse’s SQL course](https://lukebarousse.com/sql) which includes real job postings for data analyst roles.  
Using SQL, I answered the following questions:

1. 💸 What are the top-paying data analyst jobs?
2. 🧰 What skills do those jobs require?
3. 🔥 What are the most in-demand skills overall?
4. 💰 Which skills lead to better salaries?
5. 🚀 Which skills are both high-paying and high-demand?

---

## 🛠️ Tools I Used

- **SQL** – for querying the dataset  
- **PostgreSQL** – database management system  
- **VS Code** – for running queries and writing code  
- **Git & GitHub** – version control & project sharing

---

## 🔍 My Analysis & Learnings

---

### 1. Top-Paying Data Analyst Jobs

```sql
SELECT job_id, job_title, job_location, salary_year_avg
FROM job_postings_fact
WHERE job_title_short = 'Data Analyst'
  AND job_location = 'Anywhere'
  AND salary_year_avg IS NOT NULL
ORDER BY salary_year_avg DESC
LIMIT 10;
```

📌 The highest salary I found was **$650,000** (remote role!) — and yes, I double-checked!

![Top Paying Roles](assets/1_top_paying_roles.png)

---

### 2. Skills in High-Paying Jobs

I created a Common Table Expression (CTE) to filter the top 10 highest-paid roles, then pulled the related skills.

```sql
WITH top_paying_jobs AS (
  SELECT job_id, salary_year_avg
  FROM job_postings_fact
  WHERE job_title_short = 'Data Analyst'
    AND job_location = 'Anywhere'
    AND salary_year_avg IS NOT NULL
  ORDER BY salary_year_avg DESC
  LIMIT 10
)
SELECT skills
FROM top_paying_jobs
JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id;
```

---

### ✅ Top 3 Skills in These Jobs

From the top 10 highest-paying jobs:

| Skill   | Count |
|---------|-------|
| SQL     | 8     |
| Python  | 7     |
| Tableau | 6     |

These were the **most requested** skills across the top roles.  
Other popular tools: **R**, **Pandas**, **Snowflake**, **Excel**

![Top Paying Skills](assets/2_top_paying_roles_skills.png)

---

### 3. Most In-Demand Skills Overall

```sql
SELECT skills, COUNT(*) AS demand_count
FROM job_postings_fact
JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE job_title_short = 'Data Analyst'
  AND job_work_from_home = TRUE
GROUP BY skills
ORDER BY demand_count DESC
LIMIT 5;
```

| Skill   | Demand Count |
|---------|---------------|
| SQL     | 7291          |
| Excel   | 4611          |
| Python  | 4330          |
| Tableau | 3745          |
| Power BI| 2609          |

🔍 These are the **must-know tools** to break into data analysis roles.

---

### 4. Highest-Paying Skills

```sql
SELECT skills, ROUND(AVG(salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE job_title_short = 'Data Analyst'
  AND salary_year_avg IS NOT NULL
  AND job_work_from_home = TRUE
GROUP BY skills
ORDER BY avg_salary DESC
LIMIT 10;
```

📌 Top earners (average salary):

| Skill       | Avg Salary ($) |
|-------------|----------------|
| PySpark     | 208,172        |
| Bitbucket   | 189,155        |
| Couchbase   | 160,515        |
| Watson      | 160,515        |
| DataRobot   | 155,486        |

These are **less common**, but clearly **high-value** skills.

---

### 5. Best Skills to Learn (High Salary + Demand)

```sql
SELECT skills, COUNT(*), ROUND(AVG(salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE job_title_short = 'Data Analyst'
  AND salary_year_avg IS NOT NULL
  AND job_work_from_home = TRUE
GROUP BY skills
HAVING COUNT(*) > 10
ORDER BY avg_salary DESC
LIMIT 10;
```

📈 High-paying **AND** in-demand:

| Skill      | Demand | Avg Salary |
|------------|--------|------------|
| Go         | 27     | $115,320   |
| Snowflake  | 37     | $112,948   |
| Azure      | 34     | $111,225   |
| AWS        | 32     | $108,317   |
| BigQuery   | 13     | $109,654   |

---

## 💭 What I Learned

- How to use **JOINs**, **CTEs**, and **GROUP BY**  
- Getting better at debugging SQL queries  
- How to answer real questions from raw job data  
- Organizing code and results into a clean format

---

## 📈 Skills Practiced

| Skill             | Confidence |
|------------------|------------|
| SQL (CTEs, joins) | 🟢 Solid practice |
| PostgreSQL        | 🟢 Getting comfy |
| Markdown + Docs   | ✅ Portfolio ready |
| Git & GitHub      | ✅ Version control basics |
| Data Analysis     | ✨ Still learning! |

---

## 🧠 What’s Next?

- Work on visualizing results with Tableau or Power BI  
- Learn Python libraries like Pandas & NumPy  
- Try building dashboards or using Jupyter Notebooks  
- Clean up and share more projects like this one

---

## 📫 Let’s Connect!

I’m always happy to chat, get feedback, or collaborate:

- 🌐 [LinkedIn](https://www.linkedin.com/in/tanujkumai/)
- 💻 [GitHub](https://github.com/tanujkumai)
- 📧 Email: tanujkumai21@gmail.com

---

> Thanks for checking out my work — I hope this helps others learning too! 🙌
