# Fraud Detection System

This fraud detection system represents a comprehensive, production-grade solution featuring two primary components: a model training pipeline and a real-time inference pipeline. The training pipeline synchronizes synthetic transaction data from a Kafka producer to Confluent Cloud, implements advanced feature engineering (temporal, behavioral, and monetary patterns), handles class imbalance using SMOTE, and leverages MLflow for experiment tracking and model versioning. The real-time inference pipeline uses Spark Streaming to process transactions, apply the trained model, and output fraud predictions back to Kafka. 

The architecture follows modern MLOps principles with a cloud-native approach. It integrates tools like Apache Airflow for orchestration, PostgreSQL for metadata storage, MinIO for artifact storage, and distributed Spark clusters for scalable processing. With robust error handling and environment configuration, this system is built for high-stakes financial fraud detection in production environments.

---

## Table of Contents

- [Architecture](#architecture)
- [Components](#components)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
- [Configuration](#configuration)
- [Usage](#usage)
- [Development](#development)
- [Monitoring](#monitoring)
- [License](#license)

---

## Architecture

The system consists of two main pipelines:

### Training Pipeline
- Streams synthetic transaction data to Confluent Cloud Kafka.
- Performs feature engineering on temporal, behavioral, and monetary patterns.
- Handles class imbalance using SMOTE.
- Trains and evaluates machine learning models.
- Tracks experiments and model versions with MLflow.

### Inference Pipeline
- Processes real-time transaction data from Kafka using Spark Streaming.
- Applies the pre-trained model to detect fraudulent activity.
- Outputs fraud predictions to Kafka for downstream use.

---

## Components

- `airflow/`: Apache Airflow DAGs and plugins
- `config/`: Centralized configuration files
- `dags/`: DAG definitions for training and inference
- `docker-compose.yml`: Docker Compose setup
- `env/`: Environment-specific variables
- `inference/`: Real-time fraud detection pipeline
- `logs/`: Logs directory
- `mflow/`: MLflow setup for tracking and registry
- `models/`: Model training scripts and utilities
- `plugins/`: Airflow custom operators
- `producer/`: Kafka synthetic data generator for training

---

## 📦 Prerequisites

- Docker & Docker Compose
- Python 3.8+
- Confluent Cloud Kafka (or self-hosted Kafka)
- Apache Spark
- PostgreSQL
- MinIO (or any S3-compatible object storage)


---

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/yourusername/fraud-detection-system.git
cd fraud-detection-system
```
### 2. Set up environment variables
```bash
cp env/example.env .env
# Edit .env with your Kafka, DB, and MLflow credentials
```
### 3. Start the services
```
docker-compose up -d
```
### 4.  Initialize databases
```bash
./init-multiple-dbs.sh
```
### 5. Access Web Interfaces
	•	Airflow: http://localhost:3333
	•	MLflow: http://localhost:5500
	•	Spark UI: http://localhost:4040 (if running)
	•	MinIO Console: http://localhost:9000

## Configuration

Key configuration is managed via:

- `config/config.yaml`: Core system parameters
- `.env`: Environment variables (Kafka, PostgreSQL, MinIO, etc.)
- `docker-compose.yml`: Service settings

### Notable Parameters

- **Kafka Connection Details**
  - `KAFKA_BROKER`: Confluent Cloud Kafka broker URL.
  - `KAFKA_TOPIC`: Kafka topic for transaction data.
  - `KAFKA_GROUP_ID`: Consumer group ID for real-time streaming.
  
- **Database Configuration**
  - `DB_HOST`: PostgreSQL database host.
  - `DB_USER`: PostgreSQL user.
  - `DB_PASSWORD`: PostgreSQL password.
  - `DB_NAME`: PostgreSQL database name.
  
- **MinIO Configuration**
  - `MINIO_HOST`: MinIO host URL (for artifact storage).
  - `MINIO_ACCESS_KEY`: MinIO access key.
  - `MINIO_SECRET_KEY`: MinIO secret key.
  
- **Model Training Parameters**
  - `BATCH_SIZE`: Batch size for model training.
  - `EPOCHS`: Number of training epochs.
  - `LEARNING_RATE`: Learning rate for training.
  - `SMOTE_RATIO`: Ratio for SMOTE class balancing during training.
  
- **Spark Configuration**
  - `SPARK_MASTER`: URL for Spark cluster master node.
  - `SPARK_EXECUTOR_MEMORY`: Memory allocation for Spark executors.
  - `SPARK_DRIVER_MEMORY`: Memory allocation for Spark driver.
  
- **Fraud Detection Thresholds**
  - `FRAUD_THRESHOLD`: The score above which a transaction is flagged as fraudulent.

---

## Usage

### Training a New Model

1. Ensure your transaction data is available in the configured Kafka topic.
2. Trigger the training pipeline DAG in Airflow:
   ```bash
   airflow dags trigger train_fraud_detection_model
   ```
3. Monitor the training process through the Airflow UI at
   http://localhost:3333.

## Monitoring

- **Airflow UI**: Monitor pipeline execution, logs, and status.
- **MLflow UI**: Check model performance metrics and experiment history.
- **Spark UI**: View metrics related to streaming jobs and resource usage.
- **Logs Directory**: Check `logs/` for detailed logs about training, inference, and system operations.

## License

This project is licensed under the GNU GENERAL PUBLIC License - see the [LICENSE](LICENSE) file for details.
