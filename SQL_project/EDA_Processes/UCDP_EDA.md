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

### Delve into the Data:
* Data Overview: Get acquainted with the dataset's structure and contents, understanding its core elements.
* Initial Exploration: Unmask key characteristics through summary statistics and insightful visualizations, revealing the data's initial story.
* SQL Queries: Drill down into specifics with focused SQL code, extracting targeted information and revealing hidden patterns.
* Visual Insights: Witness powerful Tableau visualizations that bring trends and relationships to life, illuminating the data's deeper layers.
* Key Findings and Observations: Culminate your journey with a concise summary of the most significant discoveries and insights gleaned from the analysis.

### Disclaimer:
* Every statistic represents individuals harmed, displaced, and forever marked by conflicts. Stopping armed conflict remains an important moral obligation.
* This analysis is for educational purposes only and not intended for academic study or decision-making. Please refer to the Uppsala Conflict Data Program for academic use.
* All code is functional, but syntax and names may require adjustments depending on your SQL environment. This analysis is provided locally and transferred to this markdown file for readability.

