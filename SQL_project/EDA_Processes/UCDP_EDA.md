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
