---
sidebar_position: 1
---
# Quality Assurance Structure   

## Modes
The structure of analysis differs, depending on the type of comparisons we are considering. Note that the term 'table' here refers to table-like objects, which could be e.g. views, materialized views, table functions etc. depending on the context and platform. 

Code | Mode | Description
--- | :-- | :--
`1T` | Single Table | Analysis of single table data and metadata
`2T` | Pairwise Table Comparison | Comparison of data and metadata between two tables 
`1F` | Single Flow | Comparison of data and metadata across multiple tables in a data flow
`2F` | Pairwise Flow Comparison | Comparison of data and metadata between two multiple-table data flows 

Note that the pairwise comparisons can be extended to multiple tables or flows, however the foundational structure of the analysis will be equivalent. The pairwise flow comparison is especially useful for validating equivalence of multi-table data flows across different environments, relevant for data transformations being deployed using a continuous integration/continuous delivery/deployment (CI/CD) methodology.

## Types
The type of analysis required also informs the precise structure of logic and response.

Type | Description
--- | :--
`Value` | Point analysis of observed data or characteristics of a single table or flow
`Match` | Confirmation of whether observed data or characteristic matches between tables or flows
`Diff` | Highlighting the differences between observed data or characteristics 

## Foundations
This library is developed for Google BigQuery, which leverages the [INFORMATION_SCHEMA](https://cloud.google.com/bigquery/docs/information-schema-intro) metadata views. However equivalent foundational metadata views are available in e.g. [DuckDB](https://duckdb.org/docs/sql/meta/duckdb_table_functions.html), [Snowflake](https://docs.snowflake.com/en/sql-reference/info-schema), [Databricks](https://docs.databricks.com/en/sql/language-manual/sql-ref-information-schema.html) or [Redshift](https://docs.aws.amazon.com/redshift/latest/dg/c_intro_catalog_views.html).

The benefit of using these metadata views is that it minimised the requirement to query the underlying data, making the queries highly performant and the compute costs negligible.

The majority of metadata which is useful for quality assurance is returned from the following BigQuery views:

View | Description 
:-- | :-- | ---
[`INFORMATION_SCHEMA.SCHEMATA`](https://cloud.google.com/bigquery/docs/information-schema-datasets-schemata) | Metadata related to datasets within a single project and region
[`INFORMATION_SCHEMA.TABLES`](https://cloud.google.com/bigquery/docs/information-schema-tables) | Table metadata within a set of datasets
[`INFORMATION_SCHEMA.COLUMNS`](https://cloud.google.com/bigquery/docs/information-schema-columns) | Columns metadata within a set of datasets
[`INFORMATION_SCHEMA.PARTITIONS`](https://cloud.google.com/bigquery/docs/information-schema-partitions) | Date partition metadata within a set of tables
[`INFORMATION_SCHEMA.TABLE_OPTIONS`](https://cloud.google.com/bigquery/docs/information-schema-table-options) | Table options within a set of datasets, restructured for useability
[`INFORMATION_SCHEMA.TABLE_STORAGE`](https://cloud.google.com/bigquery/docs/information-schema-table-storage) | Table storage metadata within a set of datasets


## Analyses
Integrating metadata from the above views supports the following base-level analyses across the four modes and three types outlined above:

### Base Analysis
Analysis | 1T | 2T | 1F | 2F
:-- | --- | --- | --- | ---
Table Exists | `Value` | `Match` `Diff` | `Value` | `Match` `Diff`
Table Type | `Value` | `Match` `Diff` | `Value` | `Match` `Diff`
Table Source | `Value` | `Match` `Diff` | `Value` | `Match` `Diff`
Table Schema | `Value` | `Match` `Diff` | `Value` | `Match` `Diff`
Table Options | `Value` | `Match` `Diff` | `Value` | `Match` `Diff`
Table Shape | `Value` | `Match` `Diff` | `Value` | `Match` `Diff`
Table Partitions | `Value` | `Match` `Diff` | `Value` | `Match` `Diff`
Table Contents | `Value` | `Match` `Diff` | `Value` | `Match` `Diff`
Columns Exists | `Value` | `Match` `Diff` | `Value` | `Match` `Diff`
Column Types | `Value` | `Match` `Diff` | `Value` | `Match` `Diff`

However, the power of this approach is the composability of metadata analysis. This supports deeper analysis to support advanced quality assurance activities, root-cause analysis and data transformation performance verification.

### Advanced Analysis
Analysis | 1T | 2T | 1F | 2F
:-- | --- | --- | --- | --- 
Row Duplicates | `Value` |
Column Combination Uniqueness | `Value` |
Column Profile | `Value` |
Column Value Distribution | `Value` |
Metric Value | `Value` | `Match` `Diff` | `Value` | `Match` `Diff`
Transformation Map | | | `Value`
Transformation Dependencies | | | `Value` | `Match` `Diff`
Transformation Type Conversion Performance | | | `Value` | `Match` `Diff`
Transformation Parser Performance | | | `Value` | `Match` `Diff`
Transformation Join Performance | | | `Value` | `Match` `Diff`

## Constraints
A major SQL constraint is that the structural elements of a query (i.e. table or column references) cannot be defined dynamically, which means that it is a rigid language, requiring hard-coding of refrerences to specific projects, datasets and tables in order to access the metadata functionality. This limits its use as a functional language, composability and extensibility.

The `datatovalue-tools` library addresses this constraint, enabling SQL to be used as a functional language, supporting more dynamic use-cases in analysis and automation.

## Data to Value Tools
The Data to Value Tools (`datatovalue-tools`) BigQuery library is a functional mechanism for interacting with the BigQuery `INFORMATION_SCHEMA` in a parameterised manner, which enables querying of BigQuery metadata programmatically and dynamically. We use this to power extensible data applications which profile, monitor, verify and document data and data transformations.

Functions are deployed to all 39 BigQuery regions and multi-regions using Terraform. Version control is implemented using semantic versioning and a release-based strategy via GitHub.