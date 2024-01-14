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
|2016|97|

###### As we can see from this table, there seems to be a drop in reporting around 2016. Upon further inspection of the dataset we can find some documentation that indicates data collection ended in early 2017, so there is a possibility that data was lost. Using our intuition along with that knowledge, we will exclude 2016 from conclusions in our analysis.
---
### Average Suicide rate by country and generation
##### This query is broken down into 6 distinct tables by generation. Any country with an average of 0 is not included in the results. There are too many factors for why some generations might have a 0 suicide rate at a given generation, we will be treating this as the country was not reporting during the years this generation was alive (wether already deceased or not yet born) due to documentation amiguity.

#### G.I Generation (1901-1927)
```sql
SELECT country, AVG(suicides_no) AS avg_suicides
FROM dbo.Suicide_rates_raw
WHERE generation = 'G.I. Generation' AND suicides_no <> 0
GROUP BY country
ORDER BY avg_suicides desc;
```
|country|avg_suicides|
|---|---|
|Japan|2060|
|UnitedStates|2042|
|RussianFederation|1846|
|France|1153|
|Germany|1135|
|Ukraine|690|
|Italy|513|
|Spain|330|
|UnitedKingdom|321|
|Hungary|248|
|Argentina|193|
|Belgium|187|
|Brazil|177|
|Austria|175|
|Bulgaria|171|
|Poland|161|
|Canada|156|
|Cuba|151|
|RepublicofKorea|148|
|SriLanka|139|
|CzechRepublic|133|
|Serbia|127|
|Sweden|120|
|Romania|118|
|Netherlands|115|
|Switzerland|108|
|Australia|106|
|Portugal|94|
|Kazakhstan|86|
|Belarus|85|
|Denmark|64|
|Thailand|64|
|Finland|63|
|Mexico|63|
|Croatia|59|
|Lithuania|41|
|Norway|38|
|Greece|35|
|Chile|34|
|Uruguay|33|
|Israel|30|
|Latvia|30|
|Colombia|26|
|Singapore|24|
|Uzbekistan|24|
|PuertoRico|23|
|Slovenia|23|
|Slovakia|22|
|NewZealand|20|
|Estonia|18|
|Ireland|14|
|Kyrgyzstan|13|
|Philippines|10|
|Georgia|9|
|Ecuador|8|
|Turkmenistan|7|
|ElSalvador|6|
|Guatemala|5|
|Luxembourg|5|
|CostaRica|5|
|TrinidadandTobago|4|
|Armenia|3|
|Mauritius|3|
|Panama|3|
|SouthAfrica|3|
|Paraguay|2|
|Albania|2|
|Iceland|2|
|Azerbaijan|2|
|Macau|2|
|Guyana|2|
|Suriname|2|
|Grenada|1|
|SaintLucia|1|
|SaintVincentandGrenadines|1|
|Bahrain|1|
|Jamaica|1|
|Barbados|1|
|Kuwait|1|
|Aruba|1|
|Belize|1|
|Malta|1|
|Seychelles|1|

#### Silent Generation (1928-1945)
```sql
SELECT country, AVG(suicides_no) AS avg_suicides
FROM dbo.Suicide_rates_raw
WHERE generation = 'Silent' AND suicides_no <> 0
GROUP BY country
ORDER BY avg_suicides desc;
```
|country|avg_suicides|
|---|---|
|RussianFederation|3925|
|Japan|3632|
|UnitedStates|2792|
|Germany|1426|
|Ukraine|1299|
|France|1297|
|RepublicofKorea|918|
|Italy|559|
|Poland|467|
|Brazil|459|
|Spain|405|
|UnitedKingdom|381|
|SriLanka|333|
|Hungary|329|
|Canada|303|
|Belarus|288|
|Romania|275|
|Argentina|250|
|Kazakhstan|225|
|Belgium|220|
|Thailand|213|
|Serbia|198|
|Austria|197|
|Cuba|193|
|Australia|178|
|Bulgaria|171|
|CzechRepublic|164|
|Mexico|157|
|Netherlands|155|
|Switzerland|151|
|Sweden|144|
|Portugal|133|
|Croatia|114|
|Finland|114|
|Lithuania|113|
|Chile|88|
|Denmark|79|
|Colombia|77|
|Latvia|62|
|Uzbekistan|60|
|Slovenia|59|
|Slovakia|58|
|Uruguay|54|
|Turkey|53|
|Norway|52|
|Philippines|49|
|Greece|44|
|Estonia|34|
|NewZealand|34|
|Israel|33|
|PuertoRico|29|
|Singapore|29|
|Kyrgyzstan|27|
|Ireland|27|
|Ecuador|26|
|BosniaandHerzegovina|25|
|Georgia|18|
|ElSalvador|17|
|Montenegro|14|
|Turkmenistan|13|
|CostaRica|13|
|Guatemala|11|
|TrinidadandTobago|10|
|Armenia|9|
|Azerbaijan|9|
|Panama|9|
|SouthAfrica|9|
|Paraguay|8|
|Luxembourg|7|
|Mauritius|7|
|Albania|7|
|Nicaragua|6|
|Guyana|5|
|Suriname|4|
|Mongolia|3|
|Iceland|3|
|Barbados|2|
|Cyprus|2|
|Malta|2|
|Jamaica|2|
|CaboVerde|2|
|Fiji|2|
|UnitedArabEmirates|2|
|Macau|2|
|Qatar|1|
|Kiribati|1|
|Maldives|1|
|Belize|1|
|SaintVincentandGrenadines|1|
|AntiguaandBarbuda|1|
|Bahrain|1|
|SanMarino|1|
|Bahamas|1|
|Aruba|1|
|Kuwait|1|
|Grenada|1|
|Oman|1|
|SaintLucia|1|
|Seychelles|1|


