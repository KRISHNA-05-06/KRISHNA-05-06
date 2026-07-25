<h1 align="center">Sri Krishna Sai Kota</h1>

<p align="center">
  <b>Data &amp; AI Engineer</b><br>
  Building AI-integrated data pipelines at production scale
</p>

<p align="center">
  <a href="https://krishna-05-06.github.io"><img src="https://img.shields.io/badge/Portfolio-0B0B0B?style=for-the-badge&logo=googlechrome&logoColor=white" alt="Portfolio"></a>
  <a href="https://linkedin.com/in/srikrishnasai/"><img src="https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn"></a>
  <a href="mailto:srikrishnasaikota1@gmail.com"><img src="https://img.shields.io/badge/Email-D14836?style=for-the-badge&logo=gmail&logoColor=white" alt="Email"></a>
</p>

---

### About

Data engineer with 4+ years building large scale pipelines, currently finishing an M.S. in Computer Science at the University of South Florida.

Most of my production experience is financial data at scale: ETL on AWS processing 200M+ records daily and 100+ orchestrated jobs for a banking client. What I have been building since is the layer on top of that, using LLMs as a data engineering primitive rather than a demo, with the accuracy measured against a baseline instead of asserted.

Based in Tampa, FL and open to relocating.

---

### Featured Work

**[OncoLake](https://github.com/KRISHNA-05-06/oncolake)** &nbsp;·&nbsp; Snowflake clinical research data platform

Snowflake native platform on fully synthetic oncology data. Dual ingestion (Snowpipe event driven plus Matillion orchestrated) into one warehouse, LLM extraction from free text clinical notes, dbt marts with an SCD Type 2 patient dimension, and a Streamlit in Snowflake cohort explorer.

`Snowflake` `Matillion` `Snowpipe` `dbt` `AWS` `Claude API` `Streamlit` `Airflow`

> 5/5 dbt data quality tests passing &nbsp;·&nbsp; SCD2 dimension preserving stage progression &nbsp;·&nbsp; documented query tuning finding on when clustering helps versus hurts

<br>

**[Oncology Notes Extraction](https://github.com/KRISHNA-05-06/oncology-notes-extraction)** &nbsp;·&nbsp; Clinical NLP pipeline, benchmarked

Unstructured oncology notes to governed structured data. HIPAA Safe Harbor de identification as a hard gate, Claude API abstraction into a fixed schema, data quality gate, then Snowflake and dbt. Every accuracy claim is measured against a rule based baseline.

`Claude API` `Airflow` `Snowflake` `dbt` `ICD-O-3` `HIPAA Safe Harbor`

> **0.908 micro-F1** vs 0.884 rule based baseline &nbsp;·&nbsp; **1.000 biomarker F1** vs 0.526 baseline &nbsp;·&nbsp; 2.6 PHI identifiers scrubbed per note

<br>

**[Real-Time AI Event Intelligence](https://github.com/KRISHNA-05-06/realtime-ai-pipeline)** &nbsp;·&nbsp; Streaming anomaly detection

Containerized streaming pipeline across 13 Docker services. Kafka ingestion, ClickHouse storage, Isolation Forest anomaly detection, and LLM intent classification.

`Kafka` `ClickHouse` `Docker` `Isolation Forest` `GitHub Actions`

> **1.5M+ events** processed &nbsp;·&nbsp; **0.896 anomaly F1** &nbsp;·&nbsp; 21/21 CI tests passing

<br>

**Airflow, Deployed Two Ways** &nbsp;·&nbsp; [AWS ECS Fargate](https://github.com/KRISHNA-05-06/airflow-aws-deployment) &nbsp;|&nbsp; [GCP Compute Engine](https://github.com/KRISHNA-05-06/airflow-gcp-deployment)

The same ETL pipeline deployed with deliberately different architectures: serverless ECS Fargate with RDS and an Application Load Balancer on AWS, versus a single Compute Engine VM running docker-compose with keyless GCS auth on GCP. Built to compare the tradeoffs directly.

`Airflow 3.0` `ECS Fargate` `RDS` `ECR` `Compute Engine` `GCS` `Docker Compose`

> Traced an ALB health check crash loop to a missing security group rule &nbsp;·&nbsp; tuned Uvicorn workers to survive a 4 GB VM

<br>

<p align="left">
  <a href="https://github.com/KRISHNA-05-06/Data-Engineer-Projects"><b>See all 10 projects</b></a> including PySpark tuning (61% runtime reduction), containerized ETL, and an automated job intelligence system.
</p>

---

### Tech Stack

**Languages** &nbsp; Python &nbsp;·&nbsp; SQL &nbsp;·&nbsp; Bash

**Warehousing &amp; Big Data** &nbsp; Snowflake &nbsp;·&nbsp; ClickHouse &nbsp;·&nbsp; Redshift &nbsp;·&nbsp; PySpark &nbsp;·&nbsp; Hadoop

**Streaming &amp; Ingestion** &nbsp; Kafka &nbsp;·&nbsp; Snowpipe &nbsp;·&nbsp; Matillion &nbsp;·&nbsp; Streams &amp; Tasks

**Orchestration &amp; Modeling** &nbsp; Apache Airflow &nbsp;·&nbsp; dbt

**Cloud** &nbsp; AWS (S3, ECS Fargate, ECR, RDS, Glue, Lambda, EMR, SQS, Secrets Manager) &nbsp;·&nbsp; GCP (Compute Engine, Cloud Storage, IAM)

**AI / ML** &nbsp; Claude API &nbsp;·&nbsp; Snowflake Cortex &nbsp;·&nbsp; LLM structured extraction &nbsp;·&nbsp; Scikit-learn &nbsp;·&nbsp; Hugging Face

**DevOps** &nbsp; Docker &nbsp;·&nbsp; GitHub Actions &nbsp;·&nbsp; CI/CD

---

### Background

**M.S. Computer Science**, University of South Florida &nbsp;·&nbsp; May 2026

**Data Engineer**, Prospect Infosystem Inc. &nbsp;·&nbsp; May 2021 to Jul 2024
Client: DBS Bank. ETL pipelines on AWS processing 200M+ financial records daily, 100+ jobs orchestrated with Airflow and IBM Tivoli, Hive dimensional marts for regulatory reporting.

**Certifications** &nbsp; Building with the Claude API &nbsp;·&nbsp; Model Context Protocol (MCP) &nbsp;·&nbsp; Claude with Amazon Bedrock &nbsp;·&nbsp; Claude with Google Cloud Vertex AI &nbsp;·&nbsp; Dataquest Data Engineer

---

<p align="center">
  Open to Data Engineer, AI Engineer, and ML Engineer roles.<br>
  <a href="mailto:srikrishnasaikota1@gmail.com">srikrishnasaikota1@gmail.com</a>
</p>
