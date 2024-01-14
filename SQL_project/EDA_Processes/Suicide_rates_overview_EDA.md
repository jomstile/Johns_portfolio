# Exploring the Suicide Rates Overview Dataset

## Introduction

This document delves into an exploratory analysis of the "suicide_rates_overview" dataset, aiming to uncover patterns, trends, and insights within this sensitive and crucial topic. We'll employ a multi-pronged approach, combining descriptive statistics, SQL queries, and visualizations to create a comprehensive understanding of the data.

## Structure

* Data Overview
  * Brief description of the dataset's contents and structure.
* Initial Exploration
  * Summary statistics and visualizations to reveal key characteristics of the data.
* SQL Queries
  * Focused SQL code snippets to extract specific information and patterns.
* Visual Insights
  * Tableau visualizations embedded to illuminate trends and relationships within the data.
* Key Findings and Observations
  * Summary of the most significant discoveries and insights from the analysis.
## Ethical Considerations

Data privacy and anonymity are paramount. No personally identifiable information will be revealed.
Sensitive topics like suicide require respectful and responsible analysis.
Findings will be presented objectively, avoiding sensationalization or stigmatization.
## Disclaimer

* This analysis is for informational and educational purposes only. It's not intended to provide medical advice or replace professional consultation. If you or someone you know is struggling with suicidal thoughts, please reach out to a crisis hotline or mental health professional.
* This analysis was hosted locally and transfered into this markdown file for readability, while all code does work, syntax and names may need to be adjusted depending on how the viewer uploaded them into SQL


# Ready to begin? Let's explore.
## First lets look at what information this dataset provides

```sql 
SELECT COLUMN_NAME
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'Suicide_rates_raw"
```
|COLUMN_NAME|
|---|
|country|
|year|
|sex|
|age|
|suicides_no|
|population|
|suicides_100k_pop|
|country_year|
|HDI_for_year|
|gdp_for_year|
|gdp_per_capita|
|generation|
---

## Lets take a look at some of the more basic statisitcal breakdowns of our data
1. A break down of the countires with the highest number of records.
2. Average annual suicide rates by year.
3. Average suicide rates by gender.
4. Average Suicide rates by country and generation.
5. Countries with suicide rates above average, by year.

### Countries with the highest number of records
```sql
SELECT TOP 10 country, COUNT(*) AS total_records
FROM dbo.Suicide_rates_raw
GROUP BY country
ORDER BY total_records DESC
```
|country|total_records|
|---|---|
|Netherlands|382|
|Iceland|382|
|Mauritius|382|
|Austria|382|
|Puerto Rico|372|
|Republic of Korea|372|
|Italy|372|
|Brazil|372|
|United States|372|
|Ecuador|372|
---
### Average annual suicide rates by year
```sql
SELECT year, AVG(suicides_no) AS avg_suicides
FROM dbo.Suicide_rates_raw
GROUP BY year
ORDER BY year ASC
```
|Year|Avg.Suicides|
|---|---|
|1985|201|
|1986|209|
|1987|195|
|1988|205|
|1989|256|
|1990|251|
|1991|257|
|1992|271|
|1993|284|
|1994|284|
|1995|260|
|1996|267|
|1997|260|
|1998|263|
|1999|257|
|2000|247|
|2001|237|
|2002|248|
|2003|248|
|2004|238|
|2005|232|
|2006|228|
|2007|226|
|2008|230|
|2009|227|
|2010|226|
|2011|229|
|2012|236|
|2013|232|
|2014|238|
|2015|273|
|2016|97


