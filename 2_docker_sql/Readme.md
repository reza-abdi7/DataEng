```markdown
# NYC Taxi Data Ingestion

This project is designed to ingest NYC Taxi and Limousine Commission (TLC) Trip Record Data into a PostgreSQL database using Docker. The setup includes PostgreSQL and pgAdmin services, and a Python script to download and insert data into the database.

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Project Structure
    .
    ├── docker-compose.yaml
    ├── Dockerfile
    ├── ingest_data.py
    └── README.md


## Getting Started

Follow these steps to set up and run the project:

### 1. Clone the Repository

```bash
git clone <repository-url>
cd <repository-directory>
```

### 2. Set Up Docker Services

Ensure Docker is running on your machine, then start the services defined in `docker-compose.yaml`:

```bash
docker-compose up -d
```

This will start two services:
- **pgdatabase**: A PostgreSQL database.
- **pgadmin**: A web-based interface for managing PostgreSQL databases.

### 3. Build the Docker Image for Data Ingestion

Build the Docker image using the provided `Dockerfile`:

```bash
docker build -t ny_taxi_ingest .
```

### 4. Run the Data Ingestion Script

Run the data ingestion script using the Docker image you built. You will need to pass the required parameters to the script. Below is an example command:

```bash
docker run -it --rm ny_taxi_ingest \
    --user=root \
    --password=root \
    --host=pgdatabase \
    --port=5432 \
    --db=ny_taxi \
    --tb=yellow_tripdata \
    --url=https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2024-01.parquet
```

### 5. Access pgAdmin

Open your browser and go to `http://localhost:8080`. Log in with the following credentials:

- **Email**: admin@admin.com
- **Password**: root

Add a new server with the following details:

- **Name**: pgdatabase
- **Host**: pgdatabase
- **Port**: 5432
- **Username**: root
- **Password**: root

You should now see the `ny_taxi` database and the `yellow_tripdata` table with the ingested data.

## Script Details

The `ingest_data.py` script performs the following steps:

1. Parses command-line arguments for database connection details and the URL of the data file.
2. Downloads the data file from the provided URL.
3. Reads the data file (supports both CSV and Parquet formats).
4. Creates a table in the PostgreSQL database.
5. Inserts the data into the table in batches for efficient processing.

## Example Command

Here is an example command to run the ingestion script:

```bash
docker run -it --rm ny_taxi_ingest \
    --user=root \
    --password=root \
    --host=pgdatabase \
    --port=5432 \
    --db=ny_taxi \
    --tb=yellow_tripdata \
    --url=https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2024-01.parquet
```

## Troubleshooting

- Ensure Docker and Docker Compose are properly installed and running.
- Verify that the services are up and running using `docker-compose ps`.
- Check the logs for any errors using `docker-compose logs`.

```
