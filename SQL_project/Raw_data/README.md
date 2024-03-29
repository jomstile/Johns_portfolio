## Diving into the Data :

While both datasets offer a ton of information, not all of it aligns with our specific question. To ensure a focused analysis, I will be cleaning this data and transforming it into something that can be used.

### 1. Finding common data :

These datasets have a lot of points, but they needs to be refined to only include the countries and years they have in common (with the expception of the pre and post war time). We'll begin by pinpointing the countries that reside on both datasets and then establishing a common methology for comparing how war affect the rates of suicide.

### 2. Forging Foreign Key Bridges :

We'll add foreign key's into the data set to make the code easier to read, then we will join by both key and year.

### 3. Reshaping the UCDP date range :

The UCDP data's year ranges need reshaping. We'll transform those range of years into individual years, offering a more granular view and simplifying comparisons.

### 4. A Clearer Picture for Suicide Rates :

To eliminate potential distractions and ensure consistency, we'll remove countries within the Suicide Rates dataset that haven't experienced documented conflict. This clairifies our analysis and prevents misleading conclusions.
