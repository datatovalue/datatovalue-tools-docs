<script>
  import Mermaid from '../components/Mermaid.svelte';
</script>

# Data to Value Tools - Quality Assurance Framework

<Accordion single>
<AccordionItem title="What is the Quality Assurance Framework?">

### A programmatic approach to address Data Quality challenges

The Quality Assurance Framework from Data to Value is a metadata-driven data profiling, quality assurance, visualisation and dynamic documentation framework for Google BigQuery. It leverages native BigQuery metadata views to programmatically profile data structures, contents and relationships between data assets. It supports single table data and metadata profiling, comparison between tables, flows of data between multiple tables and comparisons between multiple-table flows. It is:

- Efficient 
- Configuration-driven
- Scalable 
- Composable 
- Extensible
- Open Source

</AccordionItem>

<AccordionItem title="Why did we build this?">

### Modelling data is complex

At Data to Value, modelling data is the fundamental method we use to turn raw data into valuable insights and actions for our clients. However, integrating multiple data sources is a complex endeavour which requires a meticulous approach to ensure that intentions and logic are represented accurately in transformation code.

### Current solutions do not meet our needs

Data profiling and quality assurance approaches are typically ad-hoc, manual, fragmented, low-level and high-effort. Existing systems are typically focused on single-table, column level checks, whilst real-world data modelling abstractions are more complex, with validation required across transformations and environments.

### We can do better

This framework and toolkit aims to change that. It enables us to verify data in a consistent, scalable, reliable and rapid manner to minimize time to value fo our clients.

</AccordionItem>

<AccordionItem title="How is it built?">

### Description
This framework leverages a combination of native BigQuery scripting functionality and [Evidence.dev](https://evidence.dev) - a modern, open-source Business Intelligence as code framework.

### Foundations
It is powered by our Data to Value Tools (`datatovalue-tools`) BigQuery library which is available across all 39 BigQuery regions and multi-regions. It has been initially implemented on a Dataform data modelling project, but is  tool-agnostic, so could be deployed on clients using e.g. DBT, SQLMesh or any other transformation tool or technique.

The library can also be used by any technology which can access BigQuery, so can be used as a foundation for automated data quality alerting or other metadata-related use-cases. Configuration is set in the Evidence project and cleanly modelled and integrated metadata is returned in a form optimised for simple usage and composability.

### Architecture

<Tabs fullWidth=true>
<Tab label="Marketers">

#### High-Level Architecture

<Mermaid id=overview-marketers>
graph BT
subgraph data_transformation["Data Transformation"]
    subgraph data["Data"]
    source_data["Source Data"] 
    bigquery["BigQuery Tables"] 
    verified_data["Transformed & Verified Data"] 
    end
end
subgraph qaf["QA Framework"]
    dtvt["Data to Value Tools"]
    dashboards["Data Quality Dashboards"]
end
source_data-->bigquery-->verified_data
data_transformation-->|"Metadata"|dtvt-->|Modelled Metadata|dashboards
</Mermaid>

</Tab>

<Tab label="Engineers">

#### High-Level Architecture

<Mermaid id=overview-engineers-high>
graph BT
subgraph data_transformation["Data Transformation"]
    subgraph data["Data"]
    source_data["Source Data"] 
    bigquery["BigQuery Tables"] 
    verified_data["Transformed & Verified Data"] 
    end
end
subgraph qaf["QA Framework"]
    dtvt["Data to Value Tools"]
    dashboards["Data Quality Dashboards"]
end
source_data-->bigquery-->verified_data
data_transformation-->|"Metadata"|dtvt-->|Modelled Metadata|dashboards
click dtvt "/overview/#data-to-value-tools" "Data to Value Tools"
click dashboards "/overview/#data-quality-dashboards" "Data Quality Dashboards"
</Mermaid>

## Data to Value Tools
The Data to Value Tools (`datatovalue-tools`) BigQuery library is a functional mechanism for interacting with the BigQuery `INFORMATION_SCHEMA` in a parameterised manner, which enables querying of BigQuery metadata programmatically and dynamically. We use this to power extensible data applications which profile, monitor, verify and document data and data transformations.

Functions are deployed to all 39 BigQuery regions and multi-regions using Terraform. Version control is implemented using semantic versioning and a release-based strategy via GitHub.

<Mermaid id=overview-engineers-dtvt-sequence>
sequenceDiagram
    autonumber
    participant Configuration
    participant Sources
    participant Metadata Models
    participant Metadata Decoders
    participant Query Functions
    Configuration ->> Sources : Environment Variables
    Sources ->> Query Functions : Arguments
    Query Functions ->> Sources : Query SQL
    Sources ->> Sources: Execute Query SQL
    Sources ->> Metadata Decoders : Metadata JSON
    Metadata Decoders ->> Sources : Base Metadata Table Objects
    Metadata Decoders -->> Metadata Models : Base Metadata Table Objects
    Metadata Models -->> Sources : Integrated Metadata Table Objects
</Mermaid>

#### Data Quality Dashboards
Data Quality Dashboards are implemented in [Evidence.dev](https://evidence.dev), which are highly customisable and interactive, and can be trivially replicated and deployed onto different clients by simply cloning a base Git repository and updating a few configuration environment variables.

Sources are defined in SQL files in the Evidence project, which are then loaded into local parquet files. This data update can be scheduled or executed via the CLI, and once loaded the data is accessible within Evidence. It is powered by [DuckDB](https://duckdb.org/) [Wasm](https://duckdb.org/docs/api/wasm/overview.html) (WebAssemby), so is incredibly fast and is powered by a [modern SQL syntax](https://duckdb.org/docs/sql/dialect/overview) with some powerful new functions for any subsequent in-app data modelling.

Evidence also renders [Mermaid](https://mermaid.js.org/) charts natively, which supports programmatic diagram generation. We currently use this to build customisable, dynamic dependency diagrams.

</Tab>

<Tab label="Executives">

# It's magic!

> “Any sufficiently advanced technology is indistinguishable from magic”
> 
> Arthur C. Clarke, 1962

# It's (nearly) free!
----
# It makes your data better!
----
More importantly, it'll make you and your team feel like [THIS](https://ai.datatovalue.com/share/caee2d69-3b98-42fa-b3b3-c9267ca0575f) about your data!


</Tab>


</Tabs>

</AccordionItem>

<AccordionItem title="How is it deployed?">

Evidenced generates a static site, so can be deployed locally, self-hosted on a variety of [cloud providers](https://docs.evidence.dev/deployment/overview/), or deployed as a managed application in the cloud. It can be a public site or private, behind Single Sign On (SSO) from Google, Okta or Microsoft. Deployment decision should be driven by technical capability, authentication requirements and cost. 

[Authentication](https://docs.evidence.dev/core-concepts/data-sources/bigquery/) can be via the CLI, Service Accounts or Oauth 2.0 depending on the deployment requirements. 

</AccordionItem>

</Accordion>
