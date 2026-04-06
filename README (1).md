# 🤖 ML Feature Store Pipeline — Feast + Airflow

Automated feature engineering pipeline that processes raw datasets, computes ML-ready features using Python & SQL, and serves them via Feast feature store. Orchestrated with Airflow and integrated with a scikit-learn model. Designed for low-latency, reusable feature serving at scale.

## 🏗️ Architecture

```
Raw Data → Feature Engineering (Python/SQL) → Feast Feature Store → Airflow Scheduler → scikit-learn Model → Predictions
```

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| Feast | Feature store (offline + online serving) |
| Apache Airflow | Pipeline orchestration |
| Python + Pandas | Feature computation |
| SQL | Feature queries & transformations |
| scikit-learn | ML model training & evaluation |
| Redis | Online feature serving (low latency) |
| Docker + Docker Compose | Local environment |

## 📁 Project Structure

```
ml-feature-store-pipeline/
├── data/
│   ├── raw/
│   │   └── sample_dataset.csv
│   └── processed/
│       └── features.parquet
├── feature_engineering/
│   ├── compute_features.py
│   ├── feature_definitions.py
│   └── sql/
│       └── feature_queries.sql
├── feature_store/
│   ├── feast_repo/
│   │   ├── feature_store.yaml
│   │   ├── feature_views.py
│   │   └── entities.py
│   └── serve_features.py
├── dags/
│   ├── feature_pipeline_dag.py
│   └── retraining_dag.py
├── model/
│   ├── train_model.py
│   ├── predict.py
│   └── evaluate.py
├── config/
│   ├── feast_config.py
│   └── airflow_config.py
├── tests/
│   ├── test_features.py
│   └── test_model.py
├── requirements.txt
├── .env.example
└── README.md
```

## ⚡ Quick Start

### Prerequisites
- Docker & Docker Compose
- Python 3.10+
- Redis (for online serving)

### 1. Clone & Setup

```bash
git clone https://github.com/yourusername/ml-feature-store-pipeline.git
cd ml-feature-store-pipeline
cp .env.example .env
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Start Docker Services

```bash
docker-compose up -d
# Starts: Redis, Airflow
```

### 4. Initialize Feast Feature Store

```bash
cd feature_store/feast_repo
feast apply          # Register feature definitions
feast materialize-incremental $(date +%Y-%m-%dT%H:%M:%S)  # Push to online store
```

### 5. Compute Features

```bash
python feature_engineering/compute_features.py
```

### 6. Train the Model

```bash
python model/train_model.py
```

### 7. Serve Predictions

```bash
python model/predict.py
```

### 8. Schedule with Airflow

Navigate to `http://localhost:8080` → Enable `feature_pipeline_dag`

## 🧠 Feature Groups

| Feature Group | Features | Update Frequency |
|---------------|----------|-----------------|
| `user_stats` | avg_order_value, purchase_count, days_since_last_order | Daily |
| `product_features` | price, category_encoded, stock_level | Hourly |
| `session_features` | clicks_last_7d, session_duration_avg | Real-time |

## 🔄 Pipeline DAG

```
compute_features → push_to_feast → validate_features → trigger_retraining → evaluate_model
```

## 🧪 Running Tests

```bash
pytest tests/ -v --cov=feature_engineering --cov=model
```

## 🌍 Environment Variables

| Variable | Description |
|----------|-------------|
| `FEAST_REPO_PATH` | Path to Feast feature repo |
| `REDIS_HOST` | Redis host for online serving |
| `REDIS_PORT` | Redis port (default: 6379) |
| `MLFLOW_TRACKING_URI` | MLflow tracking server URI |
| `DATA_SOURCE_PATH` | Path to raw input data |

## 📊 Model Performance

| Metric | Value |
|--------|-------|
| Accuracy | ~87% |
| F1 Score | ~0.85 |
| Feature Serving Latency | < 10ms (online) |

## 🤝 Contributing

Pull requests are welcome. For major changes, open an issue first.

## 📄 License

MIT
