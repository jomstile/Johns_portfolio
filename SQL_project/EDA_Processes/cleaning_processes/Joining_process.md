# Data Cleaning Challenge: Aligning Country Names Across Datasets
#### This section showcases my approach to a common data cleaning challenge: reconciling inconsistencies in country names across different datasets.

## The Problem:

#### Both the UCDP location and Suicide_rates_overview country columns contain country names, but with potentially diverse naming conventions. We need to ensure consistent representation for accurate analysis.

## Solutions:

### Two options present themselves, each with pros and cons:

1. Value Change:
* Process: Change country names in one dataset to match the other.
* Pros: Eliminates the need for a foreign key join.
* Cons:
  * Requires meticulously verifying every country change to avoid errors.
  * May introduce bias if the target dataset's naming convention is not strictly superior.
2. SQL Fuzzy Logic Join:
* Process: Create a custom SQL function to approximate country matches based on fuzzy logic (e.g., Levenshtein distance).
* Pros: Potentially faster and less error-prone than manual name changes.
* Cons:
  * Requires careful function development and validation of its matching accuracy.
  * Still necessitates creating a foreign key for verified matches.

## Portfolio Demonstration:

### While my typical approach for production would be method #1 (minimizing guesswork), this portfolio allows me to showcase both options for learning purposes:

### Method #1: Manual Value Change (Details to be added here)
### Method #2: SQL Fuzzy Logic Join (Details to be added here)
