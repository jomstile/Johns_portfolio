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

Before we can start pulling data to get a better look, we must first start eliminating countries that are not useful for our analysis. Any country that does not have a document conflict with at least one side being a government needs to be exluded. We will do this by doing
