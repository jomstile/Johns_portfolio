## Pre-processing Steps using Power Query Excel:
### The following modifications were made to the UCDP_Raw file to ensure compatibility with SQL server and align with analysis requirements:

1. Parsed 'location', 'side_a', 'side_b', 'gwno_a', 'gwno_b', 'gwno_loc', 'region': Created separate rows for each unique value within list-containing cells.
    - Although it may seem inefficient, generating additional rows guarantees seamless SQL integration and unlocks the potential for highly granular analysis.
3. Removed Columns:
   - side_a_2nd (high NULL values, irrelevant for analysis)
   - side_b_2nd (high NULL values, irrelevant for analysis)
   - side_a_id (UCDP-specific ID, GWNo used as primary identifier)
   - side_b_id (UCDP-specific ID, GWNo used as primary identifier)
   - ep_end_prec (completely NULL)
   - side_a_2nd, side_b_2nd, gwno_a_2nd, gwno_b_2nd (focus on active conflict, not supporting roles)
   - version (only useful for documentation, which is noted elsewhere in project)
## Rationale:
Rationale:
- Addressed list-based data within cells for SQL compatibility.
- Eliminated columns with high NULL values or those deemed unnecessary for the intended analysis.
- Prioritized GWNo as the primary identifier, aligning with project requirements.
- Focused on data directly relevant to active conflicts.Addressed list-based data within cells for SQL compatibility.
---
# Exploring the Uppsala Data Conflict Program's data

## Unveiling Patterns in Armed Conflict Data

This document explores the UCDP_Clean.csv dataset (Clean_data folder), dissecting early patterns, trends, and insights using a combination of descriptive statistics, targeted SQL queries, and insightful Tableau visualizations.

### Structure:
* Data Overview: A concise breakdown of the dataset's structure and key elements.
* Initial Exploration: Summary statistics and visualizations to reveal the data's initial story.
* SQL Queries: Focused code snippets to extract specific information and unveil hidden patterns.
* Visual Insights: Compelling Tableau visualizations that illuminate trends and relationships within the data.
* Key Findings: A succinct summary of the most significant discoveries and insights from the analysis.

### Disclaimer:
* Every statistic represents individuals harmed, displaced, and forever marked by conflicts. Stopping armed conflict remains an important moral obligation.
* This analysis is for educational purposes only and not intended for academic study or decision-making. Please refer to the Uppsala Conflict Data Program for academic use.
* All code is functional, but syntax and names may require adjustments depending on your SQL environment. This analysis is provided locally and transferred to this markdown file for readability.
---
# Let's explore.
## Let's look at what information this transformed dataset provides

```sql
SELECT COLUMN_NAME
FROM INFORMATION_SCHEMA.COLUMNS
WHERE TABLE_NAME = 'ucdp_v1'
```
***I have added the UCDP's codebook descriptions for what each variable name represents***
|COLUMN_NAME|CODEBOOK DESCRIPTIONS|
|--------------------------------------------------|---|
|conflict_id|The unique identifier of the conflict|
|location|The name of the country/countries whose government(s) has a primary claim to the incompatibility. Note that this is not necessarily the geographical location of the conflict|
|side_a|The name of the country/countries of Side A in a conflict. Always the government side in intrastate conflicts. Note that this is a primary party to the conflict.|
|side_b|Identifying the opposition actor or country/countries of side B in the conflict. In an intrastate conflict, this includes a military opposition organization. Note that this is a primary party to the conflict.|
|incompatibility|The main conflict issue identified per the UCDP definitions: 1= Incompatibility about territory 2= Incompatibility about government 3= Incompatibility about government AND territory|
|territory_name|The name of the territory over which the conflict is fought, provided that the incompatibility is over territory. In case the two sides use different names for the disputed territory, the name listed is the one used by the opposition organisation. One reason for this is that this is most often the name that the general public recognises. Another reason is that there are cases where the disputed territories do not have an official name.|
|year|The year of observation (1946-2022).|
|intensity_level|The intensity level in the conflict per calendar year. The intensity variable is coded in two categories:5 1= Minor: between 25 and 999 battle-related deaths in a given year. 2= War: at least 1,000 battle-related deaths in a given year|
|cumulative_intensity|This variable takes into account the temporal dimension of the conflict. It is a dummy variable that codes whether the conflict since the onset has exceeded 1,000 battle-related deaths. For conflicts with a history prior to 1946, it does not take into account the fatalities incurred in preceding years. A conflict is coded as 0 as long as it has not over time resulted in more than 1,000 battle-related deaths. Once a conflict reaches this threshold, it is coded as 1
|type_of_conflict|One of the following four types of conflict: 1 = extrasystemic (between a state and a non-state group outside its own territory, where the government side is fighting to retain control of a territory outside the state system) 2 = interstate (both sides are states in the Gleditsch and Ward membership system). 3 = intrastate (side A is always a government; side B is always one or more rebel groups; there is no involvement of foreign governments with troops, i.e. there is no side_a_2nd or side_b_2nd coded) 4 = internationalized intrastate (side A is always a government; side B is always one or more rebel groups; there is involvement of foreign governments with troops, i.e. there is at least ONE side_a_2nd or side_b_2nd coded)|
|start_date|The date, as precise as possible, of the first battle-related death in the conflict. The date is set after the conflict fulfils all criteria required in the definition of an armed conflict, except for the number of deaths.|
|start_prec|The level of precision for the initial start date|
|start_date2|The date, as precise as possible, when a given episode of conflict activity reached 25 battle-related deaths in a year. Thus, for each episode of a conflict, a new Startdate2 is coded. In case precise information is lacking, Startdate2 is by default set to 31 December. An episode is defined as continuous conflict activity. Consequently, a new episode is coded whenever a conflict restarts after one or more year(s) of inactivity.|
|start_prec2|The level of precision for startdate2.|
|ep_end|A dummy variable that codes whether the conflict is inactive the following year and an episode of the conflict thus ends. If the conflict is inactive the following year(s), this variable is coded as 1. If not, a 0 is coded. For the latest year in the dataset, it is unknown whether the conflict will be recorded as active or inactive in the following year, and the variable is always given the code 0.|
|ep_end_date|This variable is only coded in years where ep_end has the value 1. If a conflict year is followed by at least one year of conflict inactivity, the ep_end_date variable lists, as precise as possible, the date when conflict activity ended.|
|gwno_a|The Gleditsch and Ward country codes of side_a|
|gwno_b|The Gleditsch and Ward country codes of side_b.|
|gwno_loc|The Gleditsch and Ward country codes of the incompatibility.|
|region|The region of the incompatibility: 1 = Europe (GWNo: 200-399) 2= Middle East (GWNo: 630-699) 3= Asia (GWNo: 700-999) 4= Africa (GWNo: 400-626) 5= Americas (GWNo: 2-199).|

---
## Let's answer some of the following questions to begin our explortation:
1. Number of conflict per region
2. Most common type of conflict
3. How type of conflict and intensity level corrolate
4. The most frequent side_a and side_b combinations

### Number of conflicts per region
#### For the following SQL query I assisnged a row number to each conflict with its conflict_id and region group, this ensures that we can identify the first occurrence of each combination so were not counting the same conflict twice. We then filter by row_num to only count the instances where it =1. Finally we group the results by region.
```sql
WITH ranked_conflicts AS (
    SELECT *,
           ROW_NUMBER() OVER (PARTITION BY conflict_id, region ORDER BY start_date) AS row_num
    FROM dbo.ucdp_v1
)
SELECT region, COUNT(*) AS 'Number of Conflicts'
FROM ranked_conflicts
WHERE row_num = 1
GROUP BY region;
```
|region                                            |Number of Conflicts|
|--------------------------------------------------|--------------------|
|1                                                 |14                  |
|2                                                 |14                  |
|3                                                 |28                  |
|4                                                 |28                  |
|5                                                 |4                   |
---
### Next lets look at the most common types of conflict
```sql
SELECT type_of_conflict, COUNT(*) AS 'Number of conflicts'
FROM dbo.ucdp_v1
GROUP BY type_of_conflict
ORDER BY num_conflicts DESC
```
|type_of_conflict                                  |Number of conflicts|
|--------------------------------------------------|-------------|
|3                                                 |433          |
|2                                                 |230          |
|4                                                 |88           |
|1                                                 |31           |
---
### How type of conflict and intensity level corrorlate
#### For the following SQL query I took advantage of the fact that the dataset had the dummy variable 'cumulative_intensity'. I am about to group the data by the type of conflict, and then within each group it counts the number on conflicts, the using a condition sum counts those who reach cumulative_intensity =1, the percentage is made off of these two numbers and multiplied by 100.
```sql
SELECT type_of_conflict,
       COUNT(*) AS 'total conflicts',
       SUM(CASE WHEN cumulative_intensity = 1 THEN 1 ELSE 0 END) AS 'Number classified as War',
       ROUND(AVG(CASE WHEN cumulative_intensity = 1 THEN 1.0 ELSE 0.0 END) * 100, 2) AS 'Percentage of conflict that reached war'
FROM dbo.ucdp_v1
GROUP BY type_of_conflict
ORDER BY 'Percentage of conflict that reached war' DESC;
```
|type_of_conflict|total conflicts|Number classified as War|Percentage of conflict that reached war|
|------------------------------|---------------|------------------------|---------------------------------------|
|2 |230 |186|80.870000         |
|3 |433|312 |72.060000|
|4 |88  |60|68.180000          |
|1|31   |14       |45.160000  |
---
### Most frequent side_a and side_b combination
#### For this we will need a subquery to create a table to contain only unique combinations of conflict_id, side_a, and side_b. This way we only count each conflict once.
```sql
SELECT TOP 1 side_a, side_b, COUNT(*) AS 'Number of Unique Conflicts'
FROM (SELECT DISTINCT conflict_id, side_a, side_b FROM UCDP_V1) AS distinct_conflicts
GROUP BY side_a, side_b
ORDER BY 'Number of Unique Conflicts' DESC
```
|side_a|side_b|num_conflicts|
|---|---|---|
|Government of Ukraine|DPR|2|
