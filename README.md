# AeroFlow Data Platform (Skytrax Reviews Transformation)

A full-stack data transformation platform and CI/CD engineering pipeline built on Snowflake, dbt, and AWS. The project transforms raw airline review datasets into a Kimball star schema, featuring automated Slim CI/CD workflows, keyless OIDC authentication, and Infrastructure as Code (IaC).

## Core Highlights
* **Star Schema Modeling:** Engineered a Kimball-compliant star schema (5 dimensions, 1 fact table) with deterministic surrogate keys using dbt.
* **Slim CI & Incremental CD:** Configured GitHub Actions with merge-base state comparison (`--defer --favor-state`) to build and test only modified models, reducing CI compute costs.
* **Infrastructure as Code:** Fully managed Snowflake resources (warehouses, RBAC, users, schemas) and AWS infrastructure (S3, CloudFront, OIDC IAM roles) via Terraform.
* **Keyless Security:** Integrated OpenID Connect (OIDC) between GitHub Actions and AWS IAM, eliminating static credentials.
* **Auto-generated Documentation:** Automated dbt docs generation and hosting via AWS S3 and CloudFront with CDN cache invalidation on every deployment.

## Tech Stack
* **Data Warehouse:** Snowflake
* **Transformation & Orchestration:** dbt (dbt-snowflake), Apache Airflow (Astronomer, Cosmos)
* **Infrastructure as Code:** Terraform (AWS & Snowflake)
* **CI/CD & Security:** GitHub Actions, AWS OIDC, SQLFluff
* **Storage & Hosting:** AWS S3, CloudFront

## Data Architecture & Model
* **Grain:** One row per customer review submission (`review_id`).
* **Star Schema:** `fct_review`, `dim_customer`, `dim_airline`, `dim_aircraft`, `dim_location`, `dim_date`.

### Schema Layout
* `RAW`: External landing schema for raw source data.
* `SOURCE` / `INTERMEDIATE`: Staging views and normalized business logic.
* `MARTS`: Production dimensional star schema.
* `STAGING` / `DEV_*`: Scratch space for CI runs and isolated local development.

## Project Structure
```text
dbt/               dbt data transformation project & models
dbt-dags/          Astronomer / Airflow DAGs for orchestration
terraform/         Terraform scripts for Snowflake RBAC & AWS infrastructure
.github/           GitHub Actions CI/CD workflows (Slim CI & CD)
docs/              Setup guides and documentation
