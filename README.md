
# Kafka to Snowflake Data Pipeline 

This project demonstrates a real-time data pipeline that captures changes from a PostgreSQL database and streams them into Snowflake using Apache Kafka and Kafka Connect.

##  Tech Stack

- **Apache Kafka**
- **Kafka Connect**
- **Debezium PostgreSQL CDC Connector**
- **Snowflake Sink Connector**
- **PostgreSQL**
- **Docker & Docker Compose**
- **Avro / JSON**
- **PowerShell / Linux Shell**

---
![Pipeline Diagram](./Untitled%20Diagram.drawio.png)
##  Project Structure

📁 kafka-iti-lab/
├── docker-compose.yml
├── connectors/
│ └── pg/
│ └── pg-snowflake-sink.json
├── postgres/
│ └── init.sql
├── scripts/
│ └── generate_key.sh

yml
Copy
Edit

---

##  Setup Instructions

### 1. Clone the repo

```bash
git clone https://github.com/yourusername/kafka-to-snowflake.git
cd kafka-to-snowflake
2. Generate RSA Key Pair
bash
Copy
Edit
openssl genrsa -out snowflake_private_key.pem 2048
openssl rsa -in snowflake_private_key.pem -pubout -out snowflake_public_key.pem
Then, base64 encode the private key:

powershell
Copy
Edit
[Convert]::ToBase64String([System.IO.File]::ReadAllBytes("snowflake_private_key.pem"))
Use the output in your Snowflake Sink Connector.

3. Configure Snowflake
Go to Snowflake → User Settings

Paste the public key under Public Key

Note your Username and Account URL

4. Start Services
bash
Copy
Edit
docker-compose up -d
This starts:

Zookeeper

Kafka Broker

Kafka Connect

PostgreSQL

 Configure Sink Connector
Edit the pg-snowflake-sink.json to include:

snowflake.url.name

snowflake.user.name

snowflake.private.key (base64)

snowflake.private.key.passphrase (if used)

Then apply it:

powershell
Copy
Edit
Invoke-RestMethod -Uri "http://localhost:8083/connectors" `
  -Method Post `
  -ContentType "application/json" `
  -Body (Get-Content -Raw -Path "connectors/pg/pg-snowflake-sink.json")
