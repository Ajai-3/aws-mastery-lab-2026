# Analytics

Notes covering Amazon Athena, Amazon EMR, and Amazon OpenSearch.

---

## 1. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-athena.svg" width="40" height="40" valign="middle" /> Amazon Athena
- **Category**: Serverless Interactive SQL Analytics
- **Core Purpose**: Runs interactive SQL queries directly against raw files stored in Amazon S3 buckets.
- **Why Use It**: To run instant SQL queries directly on raw data files stored in S3 without provisioning database servers.

### Key Concepts
- **Serverless Analytics**: No clusters to provision or manage; pay strictly per query based on the volume of S3 data scanned.
- **S3 Data Lake Querying**: Directly queries JSON, CSV, Parquet, or ORC files in place.

---

## 2. <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/Analytics/EMR.png" width="40" height="40" valign="middle" /> Amazon EMR (Elastic MapReduce)
- **Category**: Big Data Cluster Processing
- **Core Purpose**: Managed big data processing using Apache Spark, Hadoop, Presto, and Iceberg.
- **Why Use It**: To process and transform massive petabyte-scale big data datasets using open-source frameworks.

### Key Concepts
- **Big Data Clusters**: Provisions processing clusters to parse petabytes of raw data stored in S3 and output transformed analytical datasets.

---

## 3. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-open-search.svg" width="40" height="40" valign="middle" /> Amazon OpenSearch
- **Category**: Search Engine & Log Analytics
- **Core Purpose**: Managed search engine and log analytics tool (AWS fork of Elasticsearch).
- **Why Use It**: To power fast full-text search, autocomplete search bars, and visualize real-time log telemetry.

### Key Concepts
- **Analytical & Fuzzy Search**: Used for Google-like autocomplete, full-text search, and log aggregation.
- **Kibana Integration**: Native dashboards for real-time visualization of log data and metrics.
