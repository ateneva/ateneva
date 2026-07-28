# Kappa Architecture

> is a software design pattern for big data processing that treats `all data as a continuous stream`.

Proposed in 2014 by Jay Kreps (co-creator of Apache Kafka), it was designed as a simpler alternative to the `Lambda Architecture`.

Instead of maintaining separate systems for historical batch analysis and real-time streaming, Kappa routes `100% of data through a single stream processing engine`.

---

## The Core Problem: Why Kappa Exists

Before Kappa, most big data pipelines used `Lambda Architecture`, which split data processing into two separate paths:

1. `Batch Layer:`
   > Processes huge volumes of historical data accurately (e.g., Hadoop/Spark overnight runs), but with high latency.

2. `Speed Layer:`
   > Processes live incoming data with low latency (e.g., Storm/Flink), but often with approximate results or temporary views.

While Lambda worked, it forced engineers to write, debug, and maintain `two separate codebases` doing almost the exact same business logic.

`Kappa Architecture removes the batch layer entirely.`

---

## How Kappa Architecture Works

Kappa relies on two main components: an `append-only event log` (like Apache Kafka or Apache Pulsar) and a `stream processor` (like Apache Flink or Spark Streaming).

```txt
[ Data Sources ] 
       │
       ▼
[ Immutable Event Log ] ──(Continuous Stream)──► [ Stream Processing Engine ] ──► [ Serving Layer / DB ]
  (Kafka / Pulsar)                                  (Flink / Spark)

```

1. `Single Source of Truth:`
   > All raw incoming data is ingested sequentially into an append-only log. The log is retained for as long as needed (days, months, or indefinitely).

2. `Real-time Processing:`
   > Stream processors continuously consume events from the log as they arrive and push computed results to the serving database.

3. `Reprocessing via Replay:`
   > If you update your business logic or need to recalculate historical data, you don't write a batch job. Instead, you spin up a `second instance` of your stream processor, rewind to offset zero in the event log, and replay the historical data through the new logic at high speed. Once caught up, you switch your serving layer to point to the new output and decommission the old job.

---

## Lambda vs. Kappa Comparison

| Feature | Lambda Architecture | Kappa Architecture |
| --- | --- | --- |
| `Codebases` | Two (Batch + Streaming) | `One` (Streaming only) |
| `System Complexity` | High (Dual pipelines & merged views) | `Lower` (Single pipeline) |
| `Historical Reprocessing` | Handled by batch engine | Handled by replaying log through stream engine |
| `Storage Requirement` | Dual storage (Batch storage + Event stream) | Single event stream log |

---

## Strengths and Trade-offs

### Advantages

- `Simpler Maintenance:`
  > One codebase to develop, test, and deploy.

- `Consistency:`
  > Eliminates subtle discrepancies between batch and real-time processing logic.

- `Easier Backfilling:`
  > Reprocessing historical data uses the exact same code that runs live in production.

### Trade-offs / Drawbacks

- `Storage Cost:`
  > Storing years of detailed immutable logs in high-throughput streaming systems (like Kafka) can be expensive compared to cold object storage (like S3).

- `Reprocessing Overhead:`
  > Replaying petabytes of historical stream events can be resource-intensive if the stream processing engine isn't heavily scaled for the replay task
