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
* This analysis was hosted locally and transfered into this markdown file for readability, while all code does work, syntax and names may need to be adjusted depending on how the viewer uploaded them into their SQL environment


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

## Lets take a look at some of the basic statisitcal breakdowns of our data
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
```sql
SELECT country, 
       AVG(CASE WHEN generation = 'G.I. generation' THEN suicides_no END) AS 'G.I. Generation',
       AVG(CASE WHEN generation = 'Silent' THEN suicides_no END) AS 'Silent',
       AVG(CASE WHEN generation = 'Boomers' THEN suicides_no END) AS 'Boomers',
       AVG(CASE WHEN generation = 'Generation X' THEN suicides_no END) AS 'Generation X',
	   AVG(CASE WHEN generation = 'Millenials' THEN suicides_no END) AS 'Millenials',
	   AVG(CASE WHEN generation = 'Generation Z' THEN suicides_no END) AS 'Generation Z'
FROM dbo.Suicide_rates_raw
GROUP BY country
ORDER BY country;
```
|country                                                                  |G.I. Generation|Silent|Boomers|Generation X|Millenials|Generation Z|
|-------------------------------------------------------------------------|---------------|------|-------|------------|----------|------------|
|Albania                                                                  |1              |5     |12     |11          |4         |1           |
|Antigua and Barbuda                                                      |0              |0     |0      |0           |0         |0           |
|Argentina                                                                |193            |250   |278    |200         |223       |30          |
|Armenia                                                                  |3              |9     |11     |5           |2         |0           |
|Aruba                                                                    |0              |0     |1      |0           |0         |0           |
|Australia                                                                |106            |178   |355    |243         |102       |7           |
|Austria                                                                  |175            |197   |213    |94          |30        |1           |
|Azerbaijan                                                               |2              |9     |16     |8           |4         |2           |
|Bahamas                                                                  |0              |0     |0      |0           |0         |0           |
|Bahrain                                                                  |0              |0     |3      |3           |1         |0           |
|Barbados                                                                 |0              |0     |1      |0           |0         |0           |
|Belarus                                                                  |85             |288   |512    |225         |77        |4           |
|Belgium                                                                  |187            |220   |314    |134         |44        |3           |
|Belize                                                                   |0              |0     |1      |1           |1         |0           |
|Bosnia and Herzegovina                                                   |NULL           |12    |27     |31          |3         |0           |
|Brazil                                                                   |177            |459   |1034   |806         |538       |59          |
|Bulgaria                                                                 |171            |171   |139    |54          |17        |1           |
|Cabo Verde                                                               |NULL           |1     |1      |10          |3         |1           |
|Canada                                                                   |156            |303   |658    |310         |133       |15          |
|Chile                                                                    |32             |88    |186    |137         |100       |11          |
|Colombia                                                                 |24             |77    |183    |208         |195       |39          |
|Costa Rica                                                               |3              |12    |32     |25          |18        |2           |
|Croatia                                                                  |59             |114   |137    |57          |17        |1           |
|Cuba                                                                     |151            |193   |269    |152         |28        |3           |
|Cyprus                                                                   |0              |1     |3      |4           |2         |0           |
|Czech Republic                                                           |133            |164   |270    |128         |40        |1           |
|Denmark                                                                  |64             |79    |124    |51          |11        |0           |
|Dominica                                                                 |0              |0     |0      |0           |NULL      |NULL        |
|Ecuador                                                                  |8              |26    |66     |81          |82        |27          |
|El Salvador                                                              |6              |16    |52     |73          |43        |6           |
|Estonia                                                                  |18             |34    |65     |25          |9         |0           |
|Fiji                                                                     |NULL           |1     |3      |3           |2         |0           |
|Finland                                                                  |63             |114   |208    |83          |33        |1           |
|France                                                                   |1153           |1297  |1669   |614         |166       |15          |
|Georgia                                                                  |9              |18    |20     |12          |4         |0           |
|Germany                                                                  |1135           |1426  |1718   |696         |195       |10          |
|Greece                                                                   |35             |44    |53     |31          |10        |0           |
|Grenada                                                                  |0              |0     |0      |0           |0         |0           |
|Guatemala                                                                |3              |9     |26     |29          |38        |9           |
|Guyana                                                                   |1              |5     |18     |18          |11        |2           |
|Hungary                                                                  |248            |329   |531    |175         |36        |1           |
|Iceland                                                                  |1              |2     |4      |3           |1         |0           |
|Ireland                                                                  |14             |27    |60     |46          |25        |1           |
|Israel                                                                   |30             |33    |45     |31          |18        |2           |
|Italy                                                                    |513            |559   |519    |259         |73        |4           |
|Jamaica                                                                  |0              |0     |1      |1           |0         |1           |
|Japan                                                                    |2060           |3632  |3550   |1514        |597       |36          |
|Kazakhstan                                                               |86             |225   |606    |456         |227       |34          |
|Kiribati                                                                 |0              |0     |0      |0           |0         |NULL        |
|Kuwait                                                                   |0              |0     |6      |6           |2         |0           |
|Kyrgyzstan                                                               |13             |27    |72     |57          |34        |10          |
|Latvia                                                                   |30             |62    |117    |50          |15        |0           |
|Lithuania                                                                |41             |113   |258    |118         |37        |2           |
|Luxembourg                                                               |5              |7     |10     |4           |1         |0           |
|Macau                                                                    |2              |2     |3      |1           |0         |NULL        |
|Maldives                                                                 |0              |0     |0      |0           |0         |0           |
|Malta                                                                    |0              |1     |3      |1           |0         |0           |
|Mauritius                                                                |2              |6     |19     |15          |6         |0           |
|Mexico                                                                   |63             |157   |374    |438         |411       |98          |
|Mongolia                                                                 |NULL           |3     |15     |71          |61        |NULL        |
|Montenegro                                                               |0              |6     |7      |2           |0         |0           |
|Netherlands                                                              |115            |155   |264    |122         |40        |3           |
|New Zealand                                                              |20             |34    |68     |54          |28        |3           |
|Nicaragua                                                                |NULL           |4     |15     |39          |52        |6           |
|Norway                                                                   |38             |52    |82     |49          |21        |1           |
|Oman                                                                     |NULL           |0     |0      |2           |1         |0           |
|Panama                                                                   |3              |7     |17     |16          |12        |1           |
|Paraguay                                                                 |2              |7     |16     |17          |26        |7           |
|Philippines                                                              |10             |49    |157    |187         |153       |24          |
|Poland                                                                   |161            |467   |1085   |510         |214       |11          |
|Portugal                                                                 |94             |133   |101    |49          |14        |0           |
|Puerto Rico                                                              |21             |28    |47     |21          |8         |0           |
|Qatar                                                                    |0              |0     |3      |7           |4         |0           |
|Republic of Korea                                                        |148            |918   |1273   |807         |302       |24          |
|Romania                                                                  |118            |275   |455    |206         |62        |9           |
|Russian Federation                                                       |1846           |3925  |7848   |3823        |1661      |95          |
|Saint Kitts and Nevis                                                    |0              |0     |0      |0           |0         |NULL        |
|Saint Lucia                                                              |0              |0     |1      |0           |0         |0           |
|Saint Vincent and Grenadines                                             |0              |0     |0      |0           |0         |0           |
|San Marino                                                               |0              |0     |0      |0           |0         |NULL        |
|Serbia                                                                   |127            |198   |212    |84          |23        |0           |
|Seychelles                                                               |0              |0     |0      |0           |0         |0           |
|Singapore                                                                |24             |29    |52     |25          |9         |0           |
|Slovakia                                                                 |22             |51    |125    |48          |14        |0           |
|Slovenia                                                                 |23             |59    |97     |37          |10        |0           |
|South Africa                                                             |3              |9     |41     |45          |42        |4           |
|Spain                                                                    |330            |405   |393    |237         |64        |3           |
|Sri Lanka                                                                |139            |333   |828    |583         |149       |NULL        |
|Suriname                                                                 |1              |4     |9      |8           |7         |2           |
|Sweden                                                                   |120            |144   |189    |79          |37        |2           |
|Switzerland                                                              |108            |151   |210    |92          |29        |1           |
|Thailand                                                                 |64             |213   |571    |575         |188       |13          |
|Trinidad and Tobago                                                      |3              |9     |24     |15          |7         |0           |
|Turkey                                                                   |NULL           |53    |138    |218         |155       |20          |
|Turkmenistan                                                             |7              |11    |38     |39          |24        |2           |
|Ukraine                                                                  |690            |1299  |1875   |750         |311       |25          |
|United Arab Emirates                                                     |NULL           |1     |20     |20          |6         |0           |
|United Kingdom                                                           |321            |381   |685    |393         |139       |5           |
|United States                                                            |2042           |2792  |5215   |2883        |1444      |154         |
|Uruguay                                                                  |33             |54    |57     |33          |24        |2           |
|Uzbekistan                                                               |24             |60    |202    |200         |134       |40          |
---
### Countries with suicide rates above average, by year
