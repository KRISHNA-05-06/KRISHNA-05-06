```
SnowSQL v1.4.2
Type SQL statements or !help

sri#COMPUTE_WH@PORTFOLIO.PUBLIC>
```

<table>
<tr>
<td>

**Sri Krishna Sai Kota**
Data & AI Engineer

</td>
<td>

[Portfolio](https://krishna-05-06.github.io) &nbsp;·&nbsp; [LinkedIn](https://linkedin.com/in/srikrishnasai/) &nbsp;·&nbsp; [Email](mailto:srikrishnasaikota1@gmail.com) &nbsp;·&nbsp; [All 10 projects](https://github.com/KRISHNA-05-06/Data-Engineer-Projects)

</td>
</tr>
</table>

---

```sql
SELECT * FROM engineer WHERE github = 'KRISHNA-05-06';
```

| FIELD | VALUE |
|---|---|
| ROLE | Data & AI Engineer |
| LOCATION | Tampa, FL &nbsp;`(relocatable = TRUE)` |
| EDUCATION | M.S. Computer Science, University of South Florida &nbsp;`(May 2026)` |
| EXPERIENCE | 4+ years, production data systems, banking and financial services |
| SCALE | 200M+ financial records daily, 100+ orchestrated ETL jobs |
| NOW_BUILDING | Snowflake native clinical research platform on synthetic oncology data |
| OPEN_TO | Data Engineer &nbsp;·&nbsp; AI Engineer &nbsp;·&nbsp; ML Engineer |

```
1 row returned. Elapsed 0.041s.
```

---

```sql
-- What I actually do, in one sentence
SELECT thesis FROM engineer;
```

> I take data that arrives unstructured, undocumented, or in four incompatible formats,
> and turn it into something a warehouse can govern and a researcher can query.
> When I claim an accuracy number, there is a baseline printed next to it.

---

```sql
SHOW MODELS IN DATABASE portfolio;
```

<pre>
                    ┌──────────────────────────┐
   flagship ───────►│  <b>oncolake</b>               │  Snowflake clinical platform
                    │  Snowpipe · Matillion    │  dual ingestion · SCD2 · Cortex
                    │  dbt · Streamlit · Claude│
                    └────────────┬─────────────┘
                                 │ shares extraction lineage
                                 ▼
                    ┌──────────────────────────┐
                    │  <b>oncology-notes</b>         │  0.908 micro-F1 vs 0.884 baseline
                    │  Claude · Airflow · dbt  │  HIPAA Safe Harbor gate
                    └──────────────────────────┘

                    ┌──────────────────────────┐
                    │  <b>realtime-ai-pipeline</b>   │  1.5M+ events · 0.896 anomaly F1
                    │  Kafka · ClickHouse      │  13 Docker services
                    └──────────────────────────┘

                    ┌──────────────┐    ┌──────────────┐
                    │ <b>airflow-aws</b>  │ ~~ │ <b>airflow-gcp</b>  │  same DAG, two architectures
                    │ ECS Fargate  │    │ Compute VM   │  built to compare, not assert
                    └──────────────┘    └──────────────┘

                    ┌──────────────────────────┐
                    │  <b>pyspark-optimization</b>   │  47s → 18s (61% cut)
                    │  Spark UI · Parquet      │  200 partitions → 1
                    └──────────────────────────┘
</pre>

```
6 models. Expand any row below.
```

---

<details>
<summary><b>▶ &nbsp;oncolake</b> &nbsp;<code>Snowflake · Matillion · Snowpipe · dbt · Claude API · Streamlit</code></summary>

<br/>

**[github.com/KRISHNA-05-06/oncolake](https://github.com/KRISHNA-05-06/oncolake)**

Cancer centers hold stage, site, and treatment inside free text notes. Registry reporting and cohort research need it structured and governed. This is that pipeline, on fully synthetic data.

```sql
-- the thing this repo exists to make possible
SELECT ajcc_stage, COUNT(DISTINCT patient_id) AS patients
FROM   ONCOLAKE.MARTS.COHORT_DATA_MART
WHERE  primary_site = 'lung'
GROUP  BY 1 ORDER BY 1;
```

| Layer | What happens |
|---|---|
| **RAW** | Three source shapes land as-ingested. Snowpipe (event driven, S3 to SQS) and Matillion (orchestrated) write to the same table, each path's lineage still visible in the metadata columns. |
| **STAGING** | Claude API abstracts clinical notes into schema conformant JSON. Data quality gate before anything moves. Cortex native SQL included for paid accounts. |
| **MARTS** | dbt builds `dim_patient` as SCD Type 2, so a patient at stage IIIA in January and stage IV in March keeps both rows. 5/5 tests passing. |
| **SERVING** | Streamlit in Snowflake cohort explorer. No external hosting. |

**The finding I did not expect:** I set out to prove clustering made a query faster. It did not. Low cardinality keys prune nothing, high cardinality keys trigger expensive reclustering, and measuring right after `ALTER TABLE ... CLUSTER BY` captures churn instead of steady state. Documented in `snowflake/tuning/before_after.md`, with the Query Profile screenshots that show it.

</details>

<details>
<summary><b>▶ &nbsp;oncology-notes-extraction</b> &nbsp;<code>Claude API · Airflow · Snowflake · dbt · ICD-O-3</code></summary>

<br/>

**[github.com/KRISHNA-05-06/oncology-notes-extraction](https://github.com/KRISHNA-05-06/oncology-notes-extraction)**

Most LLM extraction projects report an accuracy number with nothing to compare it to. This one runs a rule based NLP baseline on the same data, every time.

| Metric | Claude | Rule based baseline |
|---|---|---|
| Micro-F1, all fields | **0.908** | 0.884 |
| Biomarker F1 | **1.000** | 0.526 |

The gap is the whole point. Regex handles formulaic phrasing fine. It falls apart on `HER2 3+` and `EGFR exon 19 deletion`, which is exactly where the LLM earns its cost. A 2.4 point overall lift hides a 47 point lift on the field that actually varies.

HIPAA Safe Harbor de-identification runs as a hard gate before extraction, 2.6 identifiers scrubbed per note. Primary sites standardize to ICD-O-3 registry codes through a dbt seed join.

</details>

<details>
<summary><b>▶ &nbsp;realtime-ai-pipeline</b> &nbsp;<code>Kafka · ClickHouse · Docker · Isolation Forest</code></summary>

<br/>

**[github.com/KRISHNA-05-06/realtime-ai-pipeline](https://github.com/KRISHNA-05-06/realtime-ai-pipeline)**

High volume event streams need anomalies caught in flight, not in tomorrow's batch report.

```text
producer ──► Kafka topics ──► ClickHouse ──┬──► Isolation Forest  (F1 = 0.896)
             consumer groups               └──► LLM intent classification
```

1.5M+ events at sub-second latency, across 13 Docker services, 21/21 CI tests passing.

</details>

<details>
<summary><b>▶ &nbsp;airflow-aws-deployment &nbsp;~~&nbsp; airflow-gcp-deployment</b> &nbsp;<code>ECS Fargate vs Compute Engine</code></summary>

<br/>

**[AWS](https://github.com/KRISHNA-05-06/airflow-aws-deployment)** &nbsp;|&nbsp; **[GCP](https://github.com/KRISHNA-05-06/airflow-gcp-deployment)**

The same ETL pipeline, deployed twice with deliberately different architectures. Built so I could compare the tradeoffs directly instead of repeating what a blog post said.

| | AWS | GCP |
|---|---|---|
| Compute | ECS Fargate, serverless, 4 tasks | One Compute Engine VM, docker-compose |
| Metadata | Amazon RDS PostgreSQL | Containerized PostgreSQL |
| Access | Application Load Balancer | Firewall restricted port |
| Auth | IAM task roles | Keyless, VM metadata server |

**Two failures worth more than the deploys.** On AWS, the ALB health checks timed out in a loop until I traced it to a missing security group rule on 8080. On GCP, the Airflow 3 API server crash looped until I cut it to a single Uvicorn worker, because 4 GB does not fit the default.

</details>

<details>
<summary><b>▶ &nbsp;pyspark-etl-optimization</b> &nbsp;<code>PySpark · Spark UI · Parquet</code></summary>

<br/>

**[github.com/KRISHNA-05-06/PySpark-ETL-Pipeline-Optimization](https://github.com/KRISHNA-05-06/PySpark-ETL-Pipeline-Optimization)**

Read the Spark UI instead of guessing. Found redundant `.count()` actions, 200 partitions holding 75 records, and repeated full scans with no caching.

| | Before | After |
|---|---|---|
| Runtime | 47s | **18s** |
| Partitions | 200 | 1 |

Predicate pushdown, strategic caching, coalesce based repartitioning, and multiple aggregations collapsed into one `.agg()` pass.

</details>

<details>
<summary><b>▶ &nbsp;also in the warehouse</b> &nbsp;<code>4 more</code></summary>

<br/>

- **[airflow_dags](https://github.com/KRISHNA-05-06/airflow_dags)** &nbsp;·&nbsp; dynamic task mapping with `.expand()`, git-sync deploying DAGs every 30s, CI validating syntax pre-deploy
- **[PySpark-ETL-Pipeline](https://github.com/KRISHNA-05-06/PySpark-ETL-Pipeline)** &nbsp;·&nbsp; three messy order sources unified with `unionByName` and `PERMISSIVE` mode, no rows dropped
- **[python-postgres-etl-pipeline](https://github.com/KRISHNA-05-06/python-postgres-etl-pipeline)** &nbsp;·&nbsp; multi-stage Docker build, non-root user, health-gated `depends_on`
- **[ai-job-hunter](https://github.com/KRISHNA-05-06/ai-job-hunter)** &nbsp;·&nbsp; four job boards scraped on cron, LLM relevance scoring, serverless via GitHub Actions

</details>

---

```sql
SELECT * FROM experience ORDER BY start_date DESC;
```

**Prospect Infosystem Inc.** &nbsp;·&nbsp; Data Engineer &nbsp;·&nbsp; `2021-05` to `2024-07`
Client: DBS Bank, banking and financial services

| | |
|---|---|
| Volume | 200M+ financial records processed daily |
| Orchestration | 100+ ETL jobs across Apache Airflow and IBM Tivoli Workload Scheduler |
| Stack | AWS S3, Glue, Spark, Redshift, Athena |
| Modeling | Business validation rules translated into Hive dimensional marts for regulatory reporting |

---

```sql
-- Query profile: where the time actually goes
EXPLAIN SELECT depth FROM skills;
```

```text
Snowflake · Snowpipe · Streams · Tasks · Cortex   ████████████████████░░  91%
dbt · dimensional modeling · SCD2 · tests         ████████████████████░░  90%
Apache Airflow · orchestration · scheduling       ███████████████████░░░  88%
Python · SQL · PySpark · Spark UI tuning          ██████████████████░░░░  84%
Claude API · MCP · structured LLM extraction      ██████████████████░░░░  82%
AWS · S3 · Glue · Lambda · ECS · Redshift         ████████████████░░░░░░  76%
Docker · GitHub Actions · CI/CD                   ███████████████░░░░░░░  71%
Kafka · ClickHouse · streaming                    █████████████░░░░░░░░░  62%
Matillion · GCP · Terraform                       ██████████░░░░░░░░░░░░  48%
```

```
Partitions scanned: 9 of 9. No pruning. This is the honest version.
```

---

```sql
SHOW GRANTS TO ROLE sri;
```

| CREDENTIAL | ISSUER |
|---|---|
| Building with the Claude API | Anthropic |
| Introduction to Model Context Protocol (MCP) | Anthropic |
| Claude with Amazon Bedrock | Anthropic |
| Claude with Google Cloud Vertex AI | Anthropic |
| Data Engineer Certification | Dataquest |

---

```sql
-- Constraints I engineer under
ALTER TABLE my_work ADD CONSTRAINT baselines_required
  CHECK (accuracy_claim IS NULL OR baseline IS NOT NULL);

ALTER TABLE my_work ADD CONSTRAINT land_raw_model_later
  CHECK (ingest_stage = 'as-is' AND typing_stage = 'downstream');

ALTER TABLE my_work ADD CONSTRAINT tests_are_infrastructure
  CHECK (dbt_tests_passing = total_dbt_tests);

ALTER TABLE my_work ADD CONSTRAINT negative_results_count
  CHECK (finding IN ('it worked', 'it did not work and here is why'));

ALTER TABLE my_work ADD CONSTRAINT no_phi_ever
  CHECK (data_source = 'synthetic');
```

```
5 constraints added. All currently enforced.
```

---

<div align="center">

<img height="150" src="https://github-readme-stats.vercel.app/api?username=KRISHNA-05-06&show_icons=true&hide_border=true&include_all_commits=true&hide=issues&title_color=29B5E8&icon_color=29B5E8&text_color=8b949e&bg_color=0d1117" />
<img height="150" src="https://github-readme-stats.vercel.app/api/top-langs/?username=KRISHNA-05-06&layout=compact&hide_border=true&langs_count=6&title_color=29B5E8&text_color=8b949e&bg_color=0d1117" />

</div>

---

```sql
COMMIT;
```

**Open to Data Engineer, AI Engineer, and ML Engineer roles.**
[srikrishnasaikota1@gmail.com](mailto:srikrishnasaikota1@gmail.com) &nbsp;·&nbsp; [LinkedIn](https://linkedin.com/in/srikrishnasai/) &nbsp;·&nbsp; [Portfolio](https://krishna-05-06.github.io)

```
Statement executed successfully.
Session closed. Thanks for scanning the partitions.
```
