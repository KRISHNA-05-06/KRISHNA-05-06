<div align="center">

# Sri Krishna Sai Kota

**Data & AI Engineer** &nbsp;|&nbsp; Tampa, FL &nbsp;|&nbsp; M.S. Computer Science, USF (May 2026)

<a href="https://krishna-05-06.github.io"><img src="https://img.shields.io/badge/Portfolio-1F4E79?style=for-the-badge&logoColor=white"/></a>
<a href="https://linkedin.com/in/srikrishnasai/"><img src="https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white"/></a>
<a href="mailto:srikrishnasaikota1@gmail.com"><img src="https://img.shields.io/badge/Email-EA4335?style=for-the-badge&logo=gmail&logoColor=white"/></a>
<a href="https://github.com/KRISHNA-05-06/Data-Engineer-Projects"><img src="https://img.shields.io/badge/All_Projects-29B5E8?style=for-the-badge&logo=github&logoColor=white"/></a>

</div>

<br/>

I spent four years building ETL on AWS for a banking client, moving 200M+ financial records a day across 100+ orchestrated jobs. What I have been building since is the layer on top of that: pipelines where an LLM does the extraction that regex cannot, inside a warehouse that still governs the result.

Right now that means **[OncoLake](https://github.com/KRISHNA-05-06/oncolake)**, a Snowflake native clinical research platform built entirely on synthetic oncology data.

<br/>

---

## Featured work

### [OncoLake](https://github.com/KRISHNA-05-06/oncolake)
*Snowflake clinical research data platform*

Cancer centers keep stage, site, and treatment locked inside free text notes. Registry reporting and cohort research need that as structured, queryable data. OncoLake is that path end to end, on fully synthetic records.

Clinical data arrives three ways and lands in one warehouse. Lab results come in through **Snowpipe** on an S3 event and through **Matillion** on a schedule, into the same table, with each path's fingerprint still visible in the metadata columns. Clinical notes go through **Claude** and come back as schema conformant JSON. **dbt** builds the marts, including an SCD Type 2 patient dimension, so a patient at stage IIIA in January and stage IV in March keeps both rows. A **Streamlit in Snowflake** app serves the cohort, with no external hosting.

```
Clinical notes · Lab results · Pathology JSON
        │
        ├── Matillion ELT ──┐
        ├── Snowpipe ───────┼──► RAW ──► STAGING ──► MARTS ──► Streamlit
        └── COPY (VARIANT) ─┘         LLM + DQ gate   SCD2      cohort explorer
```

**5/5 dbt data quality tests passing.** The tuning experiment is the part I would talk about in an interview: I set out to prove clustering made a query faster, and it did not. Low cardinality keys prune nothing, high cardinality keys trigger expensive reclustering, and measuring immediately after `ALTER TABLE ... CLUSTER BY` captures churn rather than steady state. That negative result is documented with the Query Profile screenshots that show it.

`Snowflake` `Matillion` `Snowpipe` `dbt` `AWS` `Claude API` `Streamlit` `Airflow`

<br/>

### [Oncology Notes Extraction](https://github.com/KRISHNA-05-06/oncology-notes-extraction)
*Clinical NLP, benchmarked against a baseline*

Most LLM extraction projects report an accuracy number with nothing beside it. This one runs a rule based NLP baseline over the same notes on every run.

Claude reaches **0.908 micro-F1** against the baseline's **0.884**. On its own a 2.4 point lift is unremarkable, and that is the interesting part: the overall number hides where the work actually happens. On biomarkers the split is **1.000 versus 0.526**, because regex handles formulaic phrasing perfectly well and then falls apart on `HER2 3+` and `EGFR exon 19 deletion`. That gap is the argument for using an LLM here at all, and it only shows up because the baseline is there to expose it.

HIPAA Safe Harbor de-identification runs as a hard gate before extraction, scrubbing 2.6 identifiers per note. Primary sites standardize to ICD-O-3 registry codes through a dbt seed join.

`Claude API` `Airflow` `Snowflake` `dbt` `ICD-O-3` `HIPAA Safe Harbor`

<br/>

### [Real-Time AI Event Intelligence](https://github.com/KRISHNA-05-06/realtime-ai-pipeline)
*Streaming anomaly detection across 13 services*

High volume event streams need anomalies caught in flight, not in tomorrow's batch report. Events land through Kafka consumer groups into ClickHouse, where an Isolation Forest scores them in real time and an LLM classifies intent.

**1.5M+ events** processed at sub-second latency, **0.896 anomaly F1**, 21/21 CI tests passing across 13 Docker services.

`Kafka` `ClickHouse` `Docker` `Isolation Forest` `GitHub Actions`

<br/>

---

## More builds

<details>
<summary><b>Airflow, deployed two ways</b> &nbsp;·&nbsp; AWS ECS Fargate and GCP Compute Engine</summary>

<br/>

**[AWS deployment](https://github.com/KRISHNA-05-06/airflow-aws-deployment)** &nbsp;|&nbsp; **[GCP deployment](https://github.com/KRISHNA-05-06/airflow-gcp-deployment)**

The same ETL pipeline deployed twice with deliberately different architectures, so I could compare the tradeoffs directly instead of repeating what a blog post claimed.

On AWS: a custom Airflow 3.0 image in ECR, all four components running as serverless ECS Fargate tasks, RDS PostgreSQL for metadata, and the UI exposed through an Application Load Balancer. On GCP: one Compute Engine VM running everything through docker-compose, with keyless GCS auth via the VM's metadata server.

The two failures taught me more than the deploys. The ALB health checks timed out in a loop until I traced it to a missing security group rule on port 8080. The GCP API server crash looped until I cut it to a single Uvicorn worker, because Airflow 3's default does not fit in 4 GB.

`Airflow 3.0` `ECS Fargate` `ECR` `RDS` `ALB` `Compute Engine` `GCS` `Docker Compose`

</details>

<details>
<summary><b>PySpark performance tuning</b> &nbsp;·&nbsp; 47s to 18s</summary>

<br/>

**[github.com/KRISHNA-05-06/PySpark-ETL-Pipeline-Optimization](https://github.com/KRISHNA-05-06/PySpark-ETL-Pipeline-Optimization)**

Read the Spark UI instead of guessing at it. The bottlenecks were redundant `.count()` actions, 200 partitions holding 75 records, and repeated full scans with no caching.

Applied predicate pushdown, strategic caching, coalesce based repartitioning, and collapsed multiple aggregations into a single `.agg()` pass. Runtime dropped from **47 seconds to roughly 18**, a 61% cut, with partitions down from 200 to 1 and the small file problem gone with it.

`PySpark` `Spark UI` `Parquet` `Docker`

</details>

<details>
<summary><b>Production Airflow ETL pipelines</b> &nbsp;·&nbsp; git-sync auto-deploy</summary>

<br/>

**[github.com/KRISHNA-05-06/airflow_dags](https://github.com/KRISHNA-05-06/airflow_dags)**

Two pipelines built the way a real team ships DAGs. A multi-region market data pipeline uses dynamic task mapping with `.expand()` to run four parallel Extract, Transform, Load chains. A books pipeline pulls 50 real titles from the Open Library API and loads MySQL with a truncate-and-insert idempotency pattern.

A git-sync sidecar deploys DAGs from GitHub every 30 seconds, so no manual restarts, and GitHub Actions validates DAG syntax before anything reaches the scheduler.

`Airflow 3.0` `Docker` `MySQL` `git-sync` `GitHub Actions` `XCom`

</details>

<details>
<summary><b>Three more</b> &nbsp;·&nbsp; PySpark ETL, containerized ETL, job intelligence</summary>

<br/>

**[Grocery ETL (PySpark)](https://github.com/KRISHNA-05-06/PySpark-ETL-Pipeline)** &nbsp;·&nbsp; Three messy order sources unified with `unionByName` and `PERMISSIVE` mode so malformed rows surface instead of vanishing, then Spark SQL validation checks on price, date range, and record count.

**[Containerized ETL](https://github.com/KRISHNA-05-06/python-postgres-etl-pipeline)** &nbsp;·&nbsp; Multi-stage Docker build, non-root application user, and a PostgreSQL health check gating `depends_on: service_healthy` so the ETL never starts against a cold database.

**[AI Job Hunter](https://github.com/KRISHNA-05-06/ai-job-hunter)** &nbsp;·&nbsp; Four job boards scraped on a cron, deduplicated across sources, scored for relevance by an LLM, and delivered as an HTML digest. Fully serverless on GitHub Actions.

</details>

<br/>

---

## Tools I build with

<div align="center">

**Languages**

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=flat-square&logo=postgresql&logoColor=white)
![Bash](https://img.shields.io/badge/Bash-4EAA25?style=flat-square&logo=gnubash&logoColor=white)

**Warehouse, ingestion and modeling**

![Snowflake](https://img.shields.io/badge/Snowflake-29B5E8?style=flat-square&logo=snowflake&logoColor=white)
![dbt](https://img.shields.io/badge/dbt-FF694B?style=flat-square&logo=dbt&logoColor=white)
![Snowpipe](https://img.shields.io/badge/Snowpipe-29B5E8?style=flat-square&logo=snowflake&logoColor=white)
![Streams and Tasks](https://img.shields.io/badge/Streams_%26_Tasks-29B5E8?style=flat-square&logo=snowflake&logoColor=white)
![Matillion](https://img.shields.io/badge/Matillion-2A7DE1?style=flat-square&logo=matillion&logoColor=white)
![Redshift](https://img.shields.io/badge/Amazon_Redshift-8C4FFF?style=flat-square&logo=amazonredshift&logoColor=white)
![ClickHouse](https://img.shields.io/badge/ClickHouse-FFCC01?style=flat-square&logo=clickhouse&logoColor=black)

**Processing, streaming and orchestration**

![PySpark](https://img.shields.io/badge/PySpark-E25A1C?style=flat-square&logo=apachespark&logoColor=white)
![Hadoop](https://img.shields.io/badge/Hadoop-66CCFF?style=flat-square&logo=apachehadoop&logoColor=black)
![Kafka](https://img.shields.io/badge/Apache_Kafka-4A4A55?style=flat-square&logo=apachekafka&logoColor=white)
![Airflow](https://img.shields.io/badge/Airflow-017CEE?style=flat-square&logo=apacheairflow&logoColor=white)

**Databases**

![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat-square&logo=mysql&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-47A248?style=flat-square&logo=mongodb&logoColor=white)

**Cloud and delivery**

![AWS](https://img.shields.io/badge/AWS-FF9900?style=flat-square&logo=amazonwebservices&logoColor=white)
![GCP](https://img.shields.io/badge/GCP-4285F4?style=flat-square&logo=googlecloud&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=flat-square&logo=githubactions&logoColor=white)
![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=flat-square&logo=streamlit&logoColor=white)

<sub>AWS: S3 · Glue · Lambda · EMR · EC2 · ECS Fargate · ECR · RDS · Redshift · Athena · SQS · Secrets Manager · IAM &nbsp;&nbsp;|&nbsp;&nbsp; GCP: Compute Engine · Cloud Storage · IAM</sub>

**AI and ML**

![Claude API](https://img.shields.io/badge/Claude_API-D97757?style=flat-square&logo=anthropic&logoColor=white)
![MCP](https://img.shields.io/badge/MCP-D97757?style=flat-square&logo=anthropic&logoColor=white)
![Snowflake Cortex](https://img.shields.io/badge/Snowflake_Cortex-29B5E8?style=flat-square&logo=snowflake&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=flat-square&logo=scikitlearn&logoColor=white)
![Hugging Face](https://img.shields.io/badge/Hugging_Face-FFD21E?style=flat-square&logo=huggingface&logoColor=black)

<sub>LLM structured extraction · Isolation Forest anomaly detection · HIPAA Safe Harbor de-identification · ICD-O-3 coding</sub>

</div>

<br/>

---

## Background

**Data Engineer**, Prospect Infosystem Inc. &nbsp;·&nbsp; May 2021 to July 2024
Client: DBS Bank. Designed ETL pipelines on AWS using S3, Glue, Spark, Redshift, and Athena, processing 200M+ financial records daily. Orchestrated 100+ ETL jobs across Apache Airflow and IBM Tivoli Workload Scheduler, and translated business validation rules into Hive dimensional marts for regulatory and analytical reporting.

**M.S. Computer Science**, University of South Florida &nbsp;·&nbsp; graduating May 2026

**Certifications** &nbsp;·&nbsp; Building with the Claude API, Introduction to Model Context Protocol, Claude with Amazon Bedrock, and Claude with Google Cloud Vertex AI (Anthropic) &nbsp;·&nbsp; Data Engineer Certification (Dataquest)

<br/>

---

<div align="center">

**Open to Data Engineer, AI Engineer, and ML Engineer roles.**

[srikrishnasaikota1@gmail.com](mailto:srikrishnasaikota1@gmail.com) &nbsp;·&nbsp; [LinkedIn](https://linkedin.com/in/srikrishnasai/) &nbsp;·&nbsp; [Portfolio](https://krishna-05-06.github.io)

</div>
