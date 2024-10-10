
# ELT Project

This repository demonstrates a simple **Extract, Load, Transform (ELT)** process using Docker and PostgreSQL. The project includes a Python-based ELT script that moves data between two PostgreSQL databases in separate containers.

## Overview

The ELT process follows three stages:

1. **Extract**: Data is extracted from the source PostgreSQL database.
2. **Load**: The extracted data is loaded into the destination PostgreSQL database.
3. **Transform**: Data can be transformed after loading as needed (optional in this project).

## Project Structure

```
├── docker-compose.yaml       # Orchestration file to manage services
├── elt_script
│   ├── Dockerfile            # Dockerfile for Python ELT script
│   └── elt_script.py         # ELT process script
└── source_db_init
    └── init.sql              # SQL script to initialize source database with sample data
```

### Key Files

1. **`docker-compose.yaml`**: Defines the Docker setup for the following services:
   - `source_postgres`: Source PostgreSQL database pre-populated with sample data.
   - `destination_postgres`: Destination PostgreSQL database where the data is loaded.
   - `elt_script`: A service that runs the ELT script within a Python environment.

2. **`elt_script/Dockerfile`**: Builds the Docker image for the ELT script. It includes:
   - A Python environment.
   - PostgreSQL client utilities for database operations.
   - The ELT script (`elt_script.py`) as the default command to run upon container startup.

3. **`elt_script/elt_script.py`**: The Python script that handles the ELT process. It:
   - Waits for the source PostgreSQL service to be available.
   - Dumps the data from the source PostgreSQL database using `pg_dump`.
   - Loads the dumped data into the destination PostgreSQL database using `psql`.

4. **`source_db_init/init.sql`**: An initialization script for the source PostgreSQL database. It:
   - Creates tables (e.g., users, films, categories, actors).
   - Inserts sample data to simulate a real-world dataset.

## How It Works

1. **Container Orchestration**: `docker-compose` manages the setup of three Docker containers:
   - **Source Database**: A PostgreSQL container pre-loaded with sample data using the `init.sql` script.
   - **Destination Database**: An empty PostgreSQL container where the data will be loaded.
   - **ELT Script Container**: A Python environment that executes the ELT process, extracting data from the source database and loading it into the destination database.

2. **ELT Process**:
   - The `elt_script.py` starts by checking the availability of the source database.
   - Once the source database is up, it uses the `pg_dump` utility to export the database contents to a `.sql` file.
   - The script then imports this SQL dump into the destination database using `psql`.

3. **Database Initialization**: The `init.sql` script initializes the source PostgreSQL database by:
   - Creating tables such as `users`, `films`, `film_categories`, `actors`, and `film_actors`.
   - Populating these tables with sample data.

## Getting Started

### Prerequisites

- **Docker**: Ensure Docker is installed on your system. [Get Docker](https://docs.docker.com/get-docker/)
- **Docker Compose**: Ensure Docker Compose is installed. [Get Docker Compose](https://docs.docker.com/compose/install/)

### Setup Instructions

1. **Clone the Repository**:
   ```bash
   git clone https://github.com/yourusername/your-repo.git
   cd your-repo
   ```

2. **Run the ELT Process**:
   Start all services using Docker Compose:
   ```bash
   docker-compose up
   ```

3. **Access Databases**:
   - Source PostgreSQL database is accessible on port **5433**.
   - Destination PostgreSQL database is accessible on port **5434**.

   You can connect to these databases using any PostgreSQL client (e.g., `psql`, DBeaver, pgAdmin).

4. **Check ELT Results**:
   - After the ELT script runs, the destination database will contain the same tables and data as the source database.
   - You can verify the data by connecting to both databases and comparing the contents.

### Stopping the Services

To stop and remove the containers, use:
```bash
docker-compose down
```

## Troubleshooting

- **Database Connection Issues**: If the ELT script fails to connect to the databases, ensure that the containers are running and that the correct ports are open.
- **Docker Compose Errors**: Ensure Docker and Docker Compose are installed correctly, and you have sufficient privileges to run Docker commands.

## Future Enhancements

- Implement transformation logic after the data load step.
- Add unit tests to verify the integrity of the ELT process.
- Automate the ELT process on a schedule using a task scheduler like Airflow or Cron.
