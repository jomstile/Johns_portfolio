## Pre-processing Steps:
### The following modifications were made to the UCDP_Raw file to ensure compatibility with SQL server and align with analysis requirements:

1. Parsed location Column: Created separate rows for each unique location within list-containing cells.
2. Removed Columns:
   - side_a_2nd (high NULL values, irrelevant for analysis)
   - side_b_2nd (high NULL values, irrelevant for analysis)
   - side_a_id (UCDP-specific ID, GWNo used as primary identifier)
   - side_b_id (UCDP-specific ID, GWNo used as primary identifier)
   - ep_end_prec (completely NULL)
   - side_a_2nd, side_b_2nd, gwno_a_2nd, gwno_b_2nd (focus on active conflict, not supporting roles)
