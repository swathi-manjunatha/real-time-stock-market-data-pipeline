# 📈 Real-Time Stock Market Data Pipeline using Kafka and AWS

## 🚀 About This Project

This project builds a real-time data pipeline to collect, store, and analyze **live stock market data** using a distributed streaming architecture. It simulates a real-world stock analytics system using **Apache Kafka**, **AWS EC2**, **Amazon S3**, **AWS Glue**, **Athena**, and **QuickSight**, enabling seamless streaming, storage, and dashboard-based visualization of high-velocity stock feeds.

## 🧠 Objective

While stock data APIs like Alpha Vantage or Yahoo Finance can be polled for information, the purpose of this project is to demonstrate how to build a **scalable, cloud-native real-time data infrastructure** from scratch using open-source and AWS technologies.

---

## 🏗️ Architecture Overview

### 1. **Stock Market Data Ingestion**

* A Python producer streams real-time stock price data (simulated or from an API).
* Data is pushed to an Apache Kafka topic.

### 2. **Real-Time Data Streaming**

* **Apache Kafka**, hosted on an **AWS EC2** instance, handles the ingestion and streaming of messages.
* Kafka is configured for external access with proper networking (advertised listeners).

### 3. **Data Storage in Amazon S3**

* A Kafka consumer reads the messages and writes them to **Amazon S3** in `.json` format.
* S3 is used as a data lake, storing event-wise market data in a structured folder format.

### 4. **AWS Glue for Data Cataloging**

* An **AWS Glue Crawler** scans the S3 bucket and catalogs the schema.
* This enables serverless querying with AWS Athena.

### 5. **Data Querying with AWS Athena**

* SQL queries are run over the cataloged data directly from S3 using Athena.
* Users can analyze market patterns, volatility, or frequency by time, symbol, or price.

### 6. **Interactive Dashboards with QuickSight**

* **AWS QuickSight** connects to Athena to visualize key metrics:

  * Stock price trends
  * Volatility over time
  * Real-time fluctuations

---

## 🔧 Key Technologies

| Layer            | Technology              |
| ---------------- | ----------------------- |
| Data Ingestion   | Python + Kafka Producer |
| Streaming Engine | Apache Kafka on AWS EC2 |
| Storage          | Amazon S3               |
| Cataloging       | AWS Glue Crawler        |
| Query Engine     | AWS Athena (SQL)        |
| Visualization    | AWS QuickSight          |

---

## 🌟 Benefits

* **Real-Time Processing**: End-to-end streaming ensures zero-lag data availability.
* **Cloud-Native**: Fully deployable on AWS, scalable and cost-efficient.
* **Structured Lakehouse Model**: Combines streaming with queryable storage.
* **End-to-End Analytics**: From ingestion to dashboards in one integrated flow.

---

## 🛠️ Getting Started

To replicate this setup:

1. **Launch EC2** and install Kafka and Zookeeper.
2. Update `server.properties` to expose Kafka publicly:

   ```
   advertised.listeners=PLAINTEXT://<public-ip>:9092
   ```
3. **Open Kafka ports (9092)** via security group inbound rules.
4. Create a **Kafka topic**:

   ```bash
   bin/kafka-topics.sh --create --topic stock_live_feed --bootstrap-server <public-ip>:9092 --partitions 1 --replication-factor 1
   ```
5. **Run Producer** to stream stock data to the Kafka topic.
6. **Run Consumer** to read and push data to S3:

   ```python
   s3 = S3FileSystem(key='AWS_ACCESS_KEY', secret='AWS_SECRET_KEY')
   s3.open('s3://bucket-name/file.json', 'w')
   ```
7. **Create a Glue Crawler** to catalog data from S3.
8. **Query with Athena** using SQL.
9. **Visualize in QuickSight** by connecting to Athena.

---

## 📊 Example Queries (Athena)

```sql
-- Daily average stock price
SELECT symbol, date(time) as day, AVG(price) as avg_price
FROM stock_data
GROUP BY symbol, day
ORDER BY day DESC;
```

---

## 📌 Suggested Visuals for README

Attach these images or GIFs in your README:

1. **System Architecture Diagram** – showing flow from Producer → Kafka → Consumer → S3 → Glue → Athena → QuickSight.
2. **Kafka Producer & Consumer CLI** – screenshots during live streaming.
3. **S3 Bucket View** – showing .json files being created.
4. **Glue Table Output** – crawler run and table creation.
5. **Athena Query Console** – sample query and results.
6. **QuickSight Dashboard** – final visualization.

---

## 🔮 Future Enhancements

* Integrate with **live stock market APIs** like Alpha Vantage or Polygon.io.
* Perform **sentiment analysis** on financial news and integrate with price movements.
* Add **auto-scaling** with Kinesis for true production-grade architecture.
* Use **AWS Lambda** for real-time transformations and filtering.

---
