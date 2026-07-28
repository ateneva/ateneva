# Change Data Capture (CDC)

> is a software architecture pattern used to track, record, and stream changes made to a database in real time.
>> Instead of re-copying or querying an entire database every night to keep another system up-to-date, `CDC extracts only the specific rows that were inserted, updated, or deleted`.

---

## How It Works (Log-Based CDC)

While there are multiple ways to capture changes (like triggers or modified-timestamp queries), the industry standard is `Log-Based CDC`.

> When a database modifies data, it writes to an internal `transaction log` (such as MySQL's -binlog- or PostgreSQL's -WAL-) -before- modifying the physical table to guarantee crash recovery.
>> CDC tools continuously tap into this low-level write log without putting heavy extra load on the production database.

```txt
┌─────────────────┐       ┌─────────────────┐       ┌─────────────────┐       ┌─────────────────┐
│   Source DB     │       │   CDC Connector │       │ Message Broker  │       │ Target Systems  │
│  (e.g., Postgres)│ ───► │ (e.g., Debezium)│ ───► │  (e.g., Kafka)  │ ───► │(Data Warehouse/ │
│ Read Write Log  │       │ Parses WAL Logs │       │ Streams Events  │       │ Search Engine)  │
└─────────────────┘       └─────────────────┘       └─────────────────┘       └─────────────────┘

```

An individual event emitted by a CDC system typically includes:

- `Operation Type:`
  > `INSERT`, `UPDATE`, or `DELETE`

- `Before State:`
  > What the row looked like prior to modification (if `UPDATE` or `DELETE`)

- `After State:`
  > What the row looks like now (if `INSERT` or `UPDATE`)

- `Metadata:`
  > Sequence number, transaction ID, and precise timestamp

---

## Common Implementation Methods

| Method | How it Works | Pros | Cons |
| --- | --- | --- | --- |
| `Log-Based` -(Recommended)- | Reads database write-ahead/binary transaction logs directly. | Fastest, captures deletes, zero overhead on query engine. | Requires specific database engine permissions & setup. |
| `Trigger-Based` | Custom database triggers write changes into a "shadow" audit table. | Works on almost any DB; highly customizable. | Overhead on write performance of source DB. |
| `Query/Timestamp-Based` | Periodically queries rows where `updated_at > last_run_time`. | Easy to set up without admin privileges. | Misses deleted records, adds high CPU load during polling. |

---

## Why CDC Matters & Common Use Cases

1. `Real-time Analytics:`
   > Instead of waiting for overnight batch ETL jobs, CDC streams updates straight to platforms like Snowflake, Databricks, or BigQuery so dashboards are always current.

2. `Search & Cache Syncing:`
   > Whenever a user updates their profile in a SQL database, CDC instantly propagates that event to update Elasticsearch or a Redis cache without needing custom application-level code.

3. `Microservices (Outbox Pattern):`
   > Keeps services decoupled. When Service A writes to its database, CDC picks up the event and broadcasts it over Apache Kafka so Service B can react immediately.

4. `Zero-Downtime Database Migrations:`
   > When moving from on-premise infrastructure to the cloud, CDC keeps the old and new systems in lockstep so you can cut over without stopping traffic.
