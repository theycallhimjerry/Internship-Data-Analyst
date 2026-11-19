# Activity 1 — Review Source Documentation and Data Lineage
## Scope and method
Reviewed the four named Activity 1 materials: README.html,Source_Method_Assumption_KPI_Documentation_Standard.docx, SQL_Python_Data_Cleaning_and_Combining_Guide.docx and Data_Source_Documentation.csv. In line with the README's Foundation catalogue, compared the three Foundation extracts: Customer_Master_Snapshot.csv, Sales_Transaction_Cleaned.csv and Customer_Behaviour_Events.csv. Python/pandas was used for schema, row-count, key uniqueness, null, register-flag and customer-ID overlap checks. No data was silently changed.
## Results
- The source register contains 18,000 rows and 22 columns. `doc_id` is unique on all 18,000 rows.
- `source_name_code` has 16,351 distinct values; 1,532 codes appear more than once. It does not provide an explicit filename mapping for the three Foundation datasets.
- Register flags: 8,962 rows do not declare a primary key; 9,057 do not document join keys; 9,053 indicate incomplete lineage.
- 8,919 rows have a last-reviewed date earlier than the documentation date. This is an exception to investigate, not proof that the dates are wrong.
- `quality_score` ranges from 0.01 to 100.0 (mean 50.29); no calculation method or thresholds were found in the supplied standard.
- `extra_metric_22` is populated but is not defined in the supplied documentation.
## Dataset checks
| File | Rows | Columns | Candidate key | Result |
|---|---:|---:|---|---|
| Customer_Master_Snapshot.csv | 18,000 | 22 | customer_id | unique, non-null in this extract; inferred, not formally declared |
| Sales_Transaction_Cleaned.csv | 18,000 | 22 | transaction_id | unique, non-null in this extract; inferred, not formally declared |
| Customer_Behaviour_Events.csv | 18,000 | 22 | event_id | unique, non-null in this extract; inferred, not formally declared |

Candidate grain is provisionally one row per customer snapshot, transaction and behaviour event respectively. These grains and key roles require source-owner confirmation.

## Join and lineage findings
- Customer master IDs have **zero overlap** with the customer IDs in either sales or events. A direct customer_id join would therefore fail for the supplied extracts unless an approved ID crosswalk exists.
- Sales and events share **5,427** distinct customer IDs. Sales has **286** distinct IDs absent from events; events has **269** absent from sales. The expected unmatched rate and correct join direction are not documented.
- The register contains Yes/No indicators for primary-key, join-key and lineage status, but not the actual key column names, join definitions, lineage path or validation evidence.

## Conclusion / completion status
**The Activity 1 review and evidence outputs have been prepared.** The deliverable is a reviewed source register with source information, candidate keys, lineage/join gaps and verification requirements. However, the underlying documentation is not yet fully approved/complete: the critical mapping and customer-ID issues must be resolved by the data owner before the sources can be treated as fully verified. Do not label the candidate keys as formally declared keys or assume the ID formats can be joined.
