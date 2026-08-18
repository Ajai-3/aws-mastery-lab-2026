# Databases

Notes covering Amazon RDS, Amazon Aurora, Amazon DynamoDB, Amazon DocumentDB, Amazon Keyspaces, Amazon Neptune, AWS DMS, Amazon ElastiCache, and Amazon MemoryDB.

---

## 1. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-rds.svg" width="40" height="40" valign="middle" /> Amazon RDS (Relational Database Service)
- **Category**: Managed Relational SQL Database
- **Core Purpose**: Managed SQL database supporting MySQL, PostgreSQL, MariaDB, Oracle, and SQL Server.
- **Why Use It**: To run traditional SQL databases (MySQL, Postgres, SQL Server) without managing OS backups, patches, or setup manually.

### Key Concepts
- **Automated Management**: Handles OS patching, automated backups, and storage auto-scaling.
- **High Availability**: Multi-AZ deployments replicate data synchronously across availability zones.

---

## 2. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-aurora.svg" width="40" height="40" valign="middle" /> Amazon Aurora & Aurora Serverless
- **Category**: High-Performance Relational SQL
- **Core Purpose**: AWS-native enterprise relational database compatible with MySQL and PostgreSQL.
- **Why Use It**: For high-performance enterprise SQL speed (up to 5x faster) with auto-scaling compute that scales down to zero when idle.

### Key Concepts
- **High Performance**: Up to 5x faster than standard MySQL and 3x faster than standard PostgreSQL.
- **Aurora Serverless**: Auto-scales database compute capacity dynamically up or down, scaling all the way to **zero** during idle periods to save costs.

---

## 3. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-dynamodb.svg" width="40" height="40" valign="middle" /> Amazon DynamoDB
- **Category**: Managed NoSQL Key-Value Store
- **Core Purpose**: Fully managed serverless NoSQL database providing single-digit millisecond performance at scale.
- **Why Use It**: When you need single-digit millisecond NoSQL performance that scales infinitely with zero server management.

### Key Concepts
- **Extreme Predictability**: Optimized for key-value lookups and fast transactional read/writes.
- **Internal AWS Powerhouse**: Powers core internal Amazon services and AWS control planes.

---

## 4. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-documentdb.svg" width="40" height="40" valign="middle" /> Amazon DocumentDB
- **Category**: Managed Document NoSQL Database
- **Core Purpose**: Fully managed MongoDB-compatible document database.
- **Why Use It**: To run MongoDB document workloads reliably at scale using managed AWS infrastructure.

### Key Concepts
- **MongoDB Compatibility**: Supports existing MongoDB drivers, queries, and tools.
- **Managed Scaling**: Decouples compute and storage for elastic scaling.

---

## 5. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-keyspaces.svg" width="40" height="40" valign="middle" /> Amazon Keyspaces
- **Category**: Managed Wide-Column NoSQL Database
- **Core Purpose**: Managed Apache Cassandra-compatible database service.
- **Why Use It**: To run Apache Cassandra workloads without managing complex server clusters.

### Key Concepts
- **Cassandra Compatibility**: Run Cassandra workloads in AWS without managing Cassandra clusters.

---

## 6. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-neptune.svg" width="40" height="40" valign="middle" /> Amazon Neptune
- **Category**: Graph Database
- **Core Purpose**: High-performance graph database optimized for connected datasets.
- **Why Use It**: To store and query highly connected data like social networks, fraud detection graphs, and recommendation engines.

### Key Concepts
- **Relationship Modeling**: Ideal for social network graphs, recommendation engines, and fraud detection patterns.
- **Graph Queries**: Supports Gremlin and SPARQL query languages for quick graph traversals.

---

## 7. <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/Database/DatabaseMigrationService.png" width="40" height="40" valign="middle" /> AWS DMS (Database Migration Service)
- **Category**: Database Migration Tool
- **Core Purpose**: Migrates databases to AWS securely with minimal application downtime.
- **Why Use It**: To safely migrate existing on-premises or cloud databases to AWS with minimal application downtime.

### Key Concepts
- **Heterogeneous & Homogeneous Migrations**: Supports migrating on-premises databases (e.g., Oracle/Postgres) directly to AWS (e.g., RDS/Aurora).

---

## 8. <img src="https://raw.githubusercontent.com/gilbarbara/logos/main/logos/aws-elasticache.svg" width="40" height="40" valign="middle" /> Amazon ElastiCache
- **Category**: In-Memory Caching (Redis & Memcached)
- **Core Purpose**: Microsecond-latency in-memory caching layer to offload heavy database read queries.
- **Why Use It**: To cache database queries in memory (Redis/Memcached) for microsecond speed and reduced database load.

### Key Concepts
- **Microsecond Read Speeds**: In-memory data store using Redis or Memcached.
- **Volatile State**: If nodes fail, cache memory is purged; applications must re-hydrate the cache from downstream databases.

---

## 9. <img src="https://raw.githubusercontent.com/awslabs/aws-icons-for-plantuml/main/dist/Database/MemoryDB.png" width="40" height="40" valign="middle" /> Amazon MemoryDB
- **Category**: Persistent In-Memory Database (Redis-Compatible)
- **Core Purpose**: Ultra-fast in-memory database built on Redis with durable multi-AZ data persistence.
- **Why Use It**: When you need ultra-fast Redis performance combined with durable, multi-AZ permanent data persistence.

### Key Concepts
- **Durable Redis Engine**: Unlike ElastiCache, MemoryDB logs all writes to a durable multi-AZ transaction log so no data is lost upon node restarts.
