# Real-Time Stock Market Data Pipeline Using Kafka and AWS

### Creating an End-to-End Streaming Infrastructure for Market Data Collection and Analytics

---

## 📌 About This Project

This project demonstrates how to build a robust **real-time data pipeline** for streaming and analyzing stock market data using **Apache Kafka** and **Amazon Web Services (AWS)**. It simulates real-world stock feed analytics, illustrating how scalable, cloud-native architectures can be deployed for high-frequency data handling.

---

## ⚙️ Architecture Overview
![Architecture](https://github.com/user-attachments/assets/c5416953-a430-448b-9393-9027f544245c)

### 1. **Stock Market Data Collection**

* A Python-based Kafka Producer simulates stock price data in real time using a CSV data feed.
* Data is streamed to a Kafka topic hosted on an AWS EC2 instance.
<img width="540" alt="image" src="https://github.com/user-attachments/assets/94cf79db-ceef-473d-80da-b65a195d184c" />
<img width="540" alt="image" src="https://github.com/user-attachments/assets/b40652e5-b678-4776-b399-645ff3023feb" />

---

### 2. **Real-Time Data Streaming with Kafka on EC2**

* Apache Kafka and Zookeeper are installed on an Amazon EC2 instance.
* Kafka is configured using the EC2 public IP (`advertised.listeners`) to allow remote access.
* Kafka handles the ingestion and transmission of messages between the Producer and Consumer.
<img width="540" alt="image" src="https://github.com/user-attachments/assets/c16532dd-6666-4092-9254-cdd8f1b134c3" />
<img width="540" alt="image" src="https://github.com/user-attachments/assets/fa399b0e-dd04-4686-882d-cf7aa795d0dd" />


---

### 3. **Data Storage in Amazon S3**

* The Kafka Consumer listens to the topic and pushes incoming messages to **Amazon S3** as JSON files.
* `boto3` and `s3fs` libraries are used to connect and write to the S3 bucket securely.

<img width="540" alt="image" src="https://github.com/user-attachments/assets/7dafde2d-2c85-40f7-ab33-264daf7c2d26" />
<img width="540" alt="image" src="https://github.com/user-attachments/assets/f47cbd2a-7fe4-4f1c-af33-ff5d07b79e9d" />

---

### 4. **Data Crawling with AWS Glue**

* AWS Glue Crawler is set up to scan the S3 bucket and generate a schema.
* The crawler populates the AWS Glue Data Catalog with table definitions.
* The cataloged data becomes queryable without manual schema definition.

<img width="540" alt="image" src="https://github.com/user-attachments/assets/d9fb293a-309c-4b07-98fa-ec59d67dbb45" />

---

### 5. **Data Analysis with AWS Athena**

* Athena queries the structured stock data directly from S3 using standard SQL.
* Users can run complex analytics — e.g., daily price averages, top gainers/losers, or volatility patterns.
<img width="540" alt="image" src="https://github.com/user-attachments/assets/34ee57c5-e0b8-4939-af13-763858f7ee47" />

<img width="540" alt="image" src="https://github.com/user-attachments/assets/2444f177-1210-4c6b-a873-453fb439032a" />


---

### 6. **Visualization using AWS QuickSight**

* Amazon QuickSight connects to Athena and creates **interactive dashboards**.
* Visuals include:

  * Real-time stock trends
  * Symbol-wise comparisons
  * Historical price movements


---

## 🧱 Key Components

| Component          | Role                                                            |
| ------------------ | --------------------------------------------------------------- |
| **Apache Kafka**   | Streams real-time stock data via Producer/Consumer architecture |
| **Amazon EC2**     | Hosts Kafka and provides compute power                          |
| **Amazon S3**      | Stores event-based stock data from the Kafka Consumer           |
| **AWS Glue**       | Crawls S3 data and maintains the schema in Glue Catalog         |
| **AWS Athena**     | Enables serverless SQL querying on S3                           |
| **AWS QuickSight** | Visualizes stock trends and metrics in dashboard format         |

---

## 💡 Benefits of This Architecture

* **Scalability**: Kafka and AWS services scale with increasing data volume effortlessly.
* **Real-Time**: End-to-end streaming enables instant insights on high-frequency stock data.
* **Cloud-Native**: Fully deployable on AWS using serverless tools like Athena and Glue.
* **Low Maintenance**: No need to manage servers for Glue, Athena, or S3 – it’s automatic.
* **Structured Analytics**: Combines structured storage with SQL querying and visual dashboards.

---

## 🛠️ Kafka Setup on EC2 — Step-by-Step

```bash
# Download Kafka
wget https://downloads.apache.org/kafka/3.3.1/kafka_2.12-3.3.1.tgz
tar -xvf kafka_2.12-3.3.1.tgz
cd kafka_2.12-3.3.1

# Install Java
sudo yum install java-1.8.0-openjdk
java -version

# Start Zookeeper
bin/zookeeper-server-start.sh config/zookeeper.properties

# Start Kafka (new terminal)
export KAFKA_HEAP_OPTS="-Xmx256M -Xms128M"
bin/kafka-server-start.sh config/server.properties

# Configure public IP
sudo nano config/server.properties
# Set:
# advertised.listeners=PLAINTEXT://<your-ec2-public-ip>:9092

# Create Kafka topic
bin/kafka-topics.sh --create --topic demo_testing2 --bootstrap-server <public-ip>:9092 --replication-factor 1 --partitions 1

# Run Producer
bin/kafka-console-producer.sh --topic demo_testing2 --bootstrap-server <public-ip>:9092

# Run Consumer
bin/kafka-console-consumer.sh --topic demo_testing2 --bootstrap-server <public-ip>:9092
```

🖼️ *Suggested Image:* Kafka producer & consumer terminals side by side.

---

## 📦 Sample Athena Query

```sql
SELECT symbol, 
       date(timestamp) AS trading_day, 
       ROUND(AVG(price), 2) AS avg_daily_price
FROM stock_data
GROUP BY symbol, trading_day
ORDER BY trading_day DESC;
```

---

## 🧪 Testing & Output

After setting up the pipeline:

1. Run the Kafka producer script from Google Colab or Jupyter.
2. Ensure messages stream into S3 via the Kafka consumer.
3. Run the Glue crawler.
4. Query stock insights from Athena.
5. Visualize in QuickSight.

🖼️ *Suggested Images:*

* Google Colab screenshot (producer script)
* AWS Console (S3, Athena, QuickSight) snapshots

---

## 🔮 Future Enhancements

* Use real-time stock APIs (Polygon.io, Alpha Vantage).
* Integrate **news sentiment** analysis for stock prediction.
* Deploy a **monitoring dashboard** (Grafana + CloudWatch).
* Add **auto-scaling** with AWS Lambda/Kinesis integration.

---

## 📌 Final Thoughts

This project is a powerful demonstration of how you can build a production-style **real-time analytics pipeline** using Kafka and AWS, fully deployable on cloud infrastructure, ready to handle high-velocity financial data.

---
