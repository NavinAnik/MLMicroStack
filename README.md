# MicroML: Microservices-Based Machine Learning Platform

MicroML is a modular, scalable, and production-ready machine learning system designed using microservices and PostgreSQL. It separates each phase of the ML lifecycle into containerized services, each owning its own database and logic. This setup allows for independent development, deployment, and scaling of each component.

---

## 🔧 Architecture Overview

```
        [User Events, Products, Logs]
                    |
       ┌────────────▼────────────┐
       │  data-ingestion-service │
       └────────────┬────────────┘
                    ▼
       ┌────────────▼─────────────────────┐
       │ data-preprocessing-service       │
       └────────────┬─────────────────────┘
                    ▼
       ┌────────────▼────────────┐
       │  feature-store-service  │
       └────────────┬────────────┘
                    ▼
       ┌────────────▼────────────┐
       │ model-training-service  │
       └────────────┬────────────┘
                    ▼
       ┌────────────▼────────────┐
       │  model-serving-service  │  ←─── Client/API Gateway
       └────────────┬────────────┘
                    ▼
       ┌────────────▼────────────┐
       │ model-monitoring-service│
       └────────────┬────────────┘
                    ▼
             [ alert-service ]
```

Each service has its own **dedicated PostgreSQL database** to ensure isolation, scalability, and independent deployment.

---

## 📦 Repo Structure

```
ml-microservice-project/
├── docker-compose.yml                   # Compose file to run services + DBs
├── README.md                            # Project overview and documentation
├── .env                                 # Global env variables (non-sensitive)
│
├── inference_service/
│   ├── app/
│   │   ├── main.py                      # FastAPI app entry point
│   │   ├── model_loader.py             # Loads model.pkl
│   │   ├── schemas.py                  # Pydantic request/response models
│   │   └── db.py                       # SQLAlchemy DB connection
│   ├── model/                          # Folder for saved model
│   │   └── model.pkl
│   ├── migrations/                     # Alembic migrations
│   ├── Dockerfile                      # Build container for inference
│   ├── requirements.txt
│   └── .env                            # Local DB creds for this service
│
├── training_pipeline/
│   ├── train.py                        # Training script
│   ├── preprocess.py
│   ├── db.py                           # SQLAlchemy training DB connection
│   ├── config/
│   │   └── config.yaml
│   ├── migrations/                     # DB schema for training metadata
│   ├── Dockerfile
│   ├── requirements.txt
│   └── .env
│
├── feature_store/
│   ├── service.py
│   ├── db.py
│   ├── Dockerfile
│   └── .env
│
├── monitoring_service/
│   ├── monitor.py
│   ├── db.py
│   ├── logging.yaml
│   ├── Dockerfile
│   └── .env
│
├── data_ingestion_service/
│   ├── ingest.py
│   ├── db.py
│   ├── Dockerfile
│   └── .env
│
├── preprocessing_service/
│   ├── clean.py
│   ├── db.py
│   ├── Dockerfile
│   └── .env
│
├── orchestration/
│   ├── airflow_dag.py
│   ├── Dockerfile
│   └── requirements.txt
│
└── db/
    ├── init_ingestion.sql
    ├── init_training.sql
    ├── init_inference.sql
    ├── ...                             # One init.sql per service DB
```

---

## 🛠️ Tech Stack

| Layer               | Tool/Framework            |
|---------------------|---------------------------|
| API & Inference     | FastAPI, Uvicorn          |
| ML Training         |      |
| Containerization    | Docker, Docker Compose    |
| DB per Service      | PostgreSQL (one per svc)  |
| Orchestration       | Airflow, Prefect (optional)|
| Monitoring          | Prometheus, ELK, Custom   |
| Migration Tooling   | Alembic (per service)     |

---

## 🚀 Getting Started

1. Clone the repository:
```bash
git clone https://github.com/your-org/ml-microservice-project.git
cd ml-microservice-project
```

2. Start the entire stack:
```bash
docker-compose up --build
```

3. Access services:
- Inference: http://localhost:8000/docs
- PostgreSQL DBs: Accessible via mapped ports (5433–5438)

---

## 📄 License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

## 🤝 Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.