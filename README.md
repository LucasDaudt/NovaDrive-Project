# NovaDrive Data Engineering Project:
## Building a Data Pipeline with Postgres, AWS, Docker, Airflow, Snowflake and dbt.
## Goal of the project
We are embarking on the migration of a PostgreSQL database to a Snowflake warehouse. Our plan entails provisioning a virtual machine on AWS EC2, configuring Docker and Airflow within this environment. Our primary objective is to orchestrate the seamless transition of data from PostgreSQL to Snowflake. Subsequently, we will utilize dbt for data processing tasks, culminating in the creation of analytical layers for the data.

## Use Cases
* Data Migration: Transferring data from a PostgreSQL database to a Snowflake data warehouse to leverage Snowflake's features and scalability.

* Data Orchestration: Using Airflow to orchestrate the migration and synchronization process between PostgreSQL and Snowflake, ensuring that steps occur in a scheduled and automated manner.

* Cloud Infrastructure Provisioning: Creating and managing a virtual machine on AWS EC2 to host the Airflow and Docker runtime environment, ensuring a flexible and scalable environment for the data pipeline.

* Containerization with Docker: Utilizing Docker containers to encapsulate and deploy the necessary services, ensuring consistency and portability of the development and production environment.

* Data Transformation with dbt: Applying data transformations and modeling using dbt to create analytical layers in Snowflake, enabling advanced analysis and insights from the migrated data.

## Database Model
### Citys

| id_cidades | cidade      | id_estados | data_inclusao                 | data_atualizacao              |
|------------|-------------|------------|-------------------------------|-------------------------------|
| 1          | Rio Branco  | 1          | 2024-01-28 19:59:05.250712+00 | 2024-01-28 19:59:05.250712+00 |
| 2          | Maceió      | 2          | 2024-01-28 19:59:05.250712+00 | 2024-01-28 19:59:05.250712+00 |
| 3          | Macapá      | 3          | 2024-01-28 19:59:05.250712+00 | 2024-01-28 19:59:05.250712+00 |
| 4          | Manaus      | 4          | 2024-01-28 19:59:05.250712+00 | 2024-01-28 19:59:05.250712+00 |
| 5          | Salvador    | 5          | 2024-01-28 19:59:05.250712+00 | 2024-01-28 19:59:05.250712+00 |

### Clients

| id_clientes | cliente                | endereco                                  | id_concessionarias | data_inclusao                  | data_atualizacao               |
|-------------|------------------------|-------------------------------------------|-------------------|--------------------------------|--------------------------------|
| 61996       | Fernando Felipe Almeida | Alameda Nossa Senhora da Saúde, 6649    | 20                | 2024-02-06 21:40:01.59763+00  | 2024-02-06 21:40:01.59763+00  |
| 62756       | Nathalia Pedro Barbosa | Avenida Vicente das Estrelas, 625        | 3                 | 2024-02-09 13:00:01.90922+00  | 2024-02-09 13:00:01.90922+00  |
| 63514       | Sara Ricardo Oliveira  | Beco Ipiranga das Estrelas, 2814         | 27                | 2024-02-12 04:10:01.80445+00  | 2024-02-12 04:10:01.80445+00  |
| 64272       | Camila dos Cavalcanti  | Beco Padre da Estação, 5759             | 7                 | 2024-02-14 19:20:01.919644+00 | 2024-02-14 19:20:01.919644+00 |
| 65030       | Ana do Monteiro        | Vila Sete de Setembro da Prosperidade, 4368 | 19            | 2024-02-17 10:30:01.804419+00 | 2024-02-17 10:30:01.804419+00 |

### Concessionaire

| id_concessionarias | concessionaria                              | id_cidades | data_inclusao                  | data_atualizacao               |
|--------------------|---------------------------------------------|------------|--------------------------------|--------------------------------|
| 1                  | Concessionária NovaDrive Motors Rio Branco | 1          | 2024-01-28 19:59:18.49458+00  | 2024-01-28 19:59:18.49458+00  |
| 2                  | Concessionária NovaDrive Motors Maceió     | 2          | 2024-01-28 19:59:18.49458+00  | 2024-01-28 19:59:18.49458+00  |
| 3                  | Concessionária NovaDrive Motors Macapá     | 3          | 2024-01-28 19:59:18.49458+00  | 2024-01-28 19:59:18.49458+00  |
| 4                  | Concessionária NovaDrive Motors Manaus     | 4          | 2024-01-28 19:59:18.49458+00  | 2024-01-28 19:59:18.49458+00  |
| 5                  | Concessionária NovaDrive Motors Salvador   | 5          | 2024-01-28 19:59:18.49458+00  | 2024-01-28 19:59:18.49458+00  |

### Sellers

| id_vendedores | nome            | id_concessionarias | data_inclusao                  | data_atualizacao               |
|---------------|-----------------|--------------------|--------------------------------|--------------------------------|
| 1             | Paulo Fernandes | 25                 | 2024-01-28 19:59:31.087487+00 | 2024-01-28 19:59:31.087487+00 |
| 2             | Mariana Silva   | 25                 | 2024-01-28 19:59:31.087487+00 | 2024-01-28 19:59:31.087487+00 |
| 3             | Carlos Eduardo  | 25                 | 2024-01-28 19:59:31.087487+00 | 2024-01-28 19:59:31.087487+00 |
| 4             | Fernanda Gomes  | 25                 | 2024-01-28 19:59:31.087487+00 | 2024-01-28 19:59:31.087487+00 |
| 5             | Ricardo Almeida | 25                 | 2024-01-28 19:59:31.087487+00 | 2024-01-28 19:59:31.087487+00 |

### Sales

| id_vendas | id_veiculos | id_concessionarias | id_vendedores | id_clientes | valor_pago | data_venda             | data_inclusao          | data_atualizacao       |
|-----------|-------------|--------------------|---------------|-------------|------------|------------------------|------------------------|------------------------|
| 62007     | 1           | 28                 | 10            | 61011       | 244468.26  | 2024-02-02 21:27:39+00 | 2024-02-02 21:27:39+00 | 2024-02-02 21:27:39+00 |
| 62008     | 6           | 13                 | 11            | 61012       | 789127.72  | 2024-02-02 02:30:50+00 | 2024-02-02 02:30:50+00 | 2024-02-02 02:30:50+00 |
| 62009     | 5           | 13                 | 14            | 61013       | 307717.16  | 2024-02-02 23:06:55+00 | 2024-02-02 23:06:55+00 | 2024-02-02 23:06:55+00 |
| 62010     | 3           | 1                  | 34            | 61014       | 481370.54  | 2024-02-02 23:49:20+00 | 2024-02-02 23:49:20+00 | 2024-02-02 23:49:20+00 |
| 62011     | 5           | 2                  | 46            | 61015       | 306486.12  | 2024-02-02 17:14:45+00 | 2024-02-02 17:14:45+00 | 2024-02-02 17:14:45+00 |

### States

| id_estados | estado   | sigla | data_inclusao                  | data_atualizacao               |
|------------|----------|-------|--------------------------------|--------------------------------|
| 1          | Acre     | AC    | 2024-01-28 19:58:55.37794+00  | 2024-01-28 19:58:55.37794+00  |
| 2          | Alagoas  | AL    | 2024-01-28 19:58:55.37794+00  | 2024-01-28 19:58:55.37794+00  |
| 3          | Amapá    | AP    | 2024-01-28 19:58:55.37794+00  | 2024-01-28 19:58:55.37794+00  |
| 4          | Amazonas | AM    | 2024-01-28 19:58:55.37794+00  | 2024-01-28 19:58:55.37794+00  |
| 5          | Bahia    | BA    | 2024-01-28 19:58:55.37794+00  | 2024-01-28 19:58:55.37794+00  |

### Vehicles

| id_veiculos | nome            | tipo                   | valor     | data_atualizacao               | data_inclusao                  |
|-------------|-----------------|------------------------|-----------|--------------------------------|--------------------------------|
| 1           | AgileXplorer    | SUV Compacta           | 250000.00 | 2024-01-28 19:58:27.84701+00 | 2024-01-28 19:58:27.84701+00 |
| 2           | VoyageRoamer    | SUV Média              | 350000.00 | 2024-01-28 19:58:27.84701+00 | 2024-01-28 19:58:27.84701+00 |
| 3           | EcoPrestige     | SUV Premium Híbrida    | 500000.00 | 2024-01-28 19:58:27.84701+00 | 2024-01-28 19:58:27.84701+00 |
| 4           | WorkMaster      | Camionete Média        | 280000.00 | 2024-01-28 19:58:27.84701+00 | 2024-01-28 19:58:27.84701+00 |
| 5           | DoubleDuty      | Camionete Cabine Dupla | 320000.00 | 2024-01-28 19:58:27.84701+00 | 2024-01-28 19:58:27.84701+00 |

## Installation of Docker and Airflow on an Ubuntu VM.

#1- Update the APT package list:
```
sudo apt-get update
```
#2- Install necessary packages to add a new repository via HTTPS:
```
sudo apt-get install ca-certificates curl gnupg lsb-release
```

#3- Create directory to store repository keys
```
sudo mkdir -m 0755 -p /etc/apt/keyrings
```

#4- Add the Docker repository's GPG key:
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

#5- Add Docker repository to APT sources:
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

#6- Update the package list after adding the new Docker repository
```
sudo apt-get update
```

#7- Install Docker and components
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

#8- Download the Airflow docker-compose.yaml file:
```
curl -LfO 'https://airflow.apache.org/docs/apache-airflow/stable/docker-compose.yaml'
```

#9- Create directories for DAGs, logs, and plugins:
```
mkdir -p ./dags ./logs ./plugins
```

#10- Create a .env file with the user UID, used by docker for permissions
```
echo -e "AIRFLOW_UID=$(id -u)" > .env
```

#11- Start airflow
```
sudo docker compose up airflow-init
```

#12- Start Airflow in detached mode
```
sudo docker compose up -d
```

## Creating the necessary objects in Snowflake

```
create database novadrive;
create schema stage;
 
CREATE WAREHOUSE DEFAULT_WH;
 
CREATE TABLE veiculos (
    id_veiculos INTEGER,
    nome VARCHAR(255) NOT NULL,
    tipo VARCHAR(100) NOT NULL,
    valor DECIMAL(10, 2) NOT NULL,
    data_atualizacao TIMESTAMP_LTZ,
    data_inclusao TIMESTAMP_LTZ
);
 
CREATE TABLE estados (
    id_estados INTEGER,
    estado VARCHAR(100) NOT NULL,
    sigla CHAR(2) NOT NULL,
    data_inclusao TIMESTAMP_LTZ,
    data_atualizacao TIMESTAMP_LTZ
);
 
CREATE TABLE cidades (
    id_cidades INTEGER,
    cidade VARCHAR(255) NOT NULL,
    id_estados INTEGER NOT NULL,
    data_inclusao TIMESTAMP_LTZ,
    data_atualizacao TIMESTAMP_LTZ
 
);
 
CREATE TABLE concessionarias (
    id_concessionarias INTEGER,
    concessionaria VARCHAR(255) NOT NULL,
    id_cidades INTEGER NOT NULL,
    data_inclusao TIMESTAMP_LTZ,
    data_atualizacao TIMESTAMP_LTZ
);
 
CREATE TABLE vendedores (
    id_vendedores INTEGER,
    nome VARCHAR(255) NOT NULL,
    id_concessionarias INTEGER NOT NULL,
    data_inclusao TIMESTAMP_LTZ,
    data_atualizacao TIMESTAMP_LTZ
);
 
CREATE TABLE clientes (
    id_clientes INTEGER,
    cliente VARCHAR(255) NOT NULL,
    endereco TEXT NOT NULL,
    id_concessionarias INTEGER NOT NULL,
    data_inclusao TIMESTAMP_LTZ,
    data_atualizacao TIMESTAMP_LTZ
);
 
CREATE TABLE vendas (
    id_vendas INTEGER,
    id_veiculos INTEGER NOT NULL,
    id_concessionarias INTEGER NOT NULL,
    id_vendedores INTEGER NOT NULL,
    id_clientes INTEGER NOT NULL,
    valor_pago DECIMAL(10, 2) NOT NULL,
    data_venda TIMESTAMP_LTZ,
    data_inclusao TIMESTAMP_LTZ,
    data_atualizacao TIMESTAMP_LTZ
);
```

## Creation of the DAG in Airflow
```
from datetime import datetime, timedelta
from airflow.decorators import dag, task
from airflow.providers.postgres.hooks.postgres import PostgresHook
from airflow.providers.snowflake.hooks.snowflake import SnowflakeHook
 
default_args = {
    'owner': 'airflow',
    'depends_on_past': False,
    'start_date': datetime(2024, 1, 1),
    'email_on_failure': False,
    'email_on_retry': False,
    'retries': 0,
    'retry_delay': timedelta(minutes=1),
}
 
@dag(
    dag_id='postgres_to_snowflake',
    default_args=default_args,
    description='Load data incrementally from Postgres to Snowflake',
    schedule_interval=timedelta(days=1),
    catchup=False
)
def postgres_to_snowflake_etl():
    table_names = ['veiculos', 'estados', 'cidades', 'concessionarias', 'vendedores', 'clientes', 'vendas']
 
    for table_name in table_names:
        @task(task_id=f'get_max_id_{table_name}')
        def get_max_primary_key(table_name: str):
            with SnowflakeHook(snowflake_conn_id='snowflake').get_conn() as conn:
                with conn.cursor() as cursor:
                    cursor.execute(f"SELECT MAX(ID_{table_name}) FROM {table_name}")
                    max_id = cursor.fetchone()[0]
                    return max_id if max_id is not None else 0
 
        @task(task_id=f'load_data_{table_name}')
        def load_incremental_data(table_name: str, max_id: int):
            with PostgresHook(postgres_conn_id='postgres').get_conn() as pg_conn:
                with pg_conn.cursor() as pg_cursor:
                    primary_key = f'ID_{table_name}'
                    
                    pg_cursor.execute(f"SELECT column_name FROM information_schema.columns WHERE table_name = '{table_name}'")
                    columns = [row[0] for row in pg_cursor.fetchall()]
                    columns_list_str = ', '.join(columns)
                    placeholders = ', '.join(['%s'] * len(columns))
                    
                    pg_cursor.execute(f"SELECT {columns_list_str} FROM {table_name} WHERE {primary_key} > {max_id}")
                    rows = pg_cursor.fetchall()
                    
                    with SnowflakeHook(snowflake_conn_id='snowflake').get_conn() as sf_conn:
                        with sf_conn.cursor() as sf_cursor:
                            insert_query = f"INSERT INTO {table_name} ({columns_list_str}) VALUES ({placeholders})"
                            for row in rows:
                                sf_cursor.execute(insert_query, row)
 
        max_id = get_max_primary_key(table_name)
        load_incremental_data(table_name, max_id)
 
postgres_to_snowflake_etl_dag = postgres_to_snowflake_etl()
```
## Running the DAG
After establishing connections between Postgres and Airflow, as well as Snowflake and Airflow, I am ready to run the DAG.

![image](https://github.com/LucasDaudt/NovaDrive-Project/assets/104397651/c2ea5934-748d-4020-bad8-5173d2690c52)

## Transforming data with dbt
All transformations made will be in the dbt folder.
Four models were created:
* Stage (STG)
* Dimension (DIM)
* Fact (FCT)
* Analises (ANL)
![dbt-dag](https://github.com/LucasDaudt/NovaDrive-Project/assets/104397651/b89457be-c467-4dc6-8707-c0b79da6a065)

## Possible alternatives.
Other ETL tools:
* Talend
* Apache NiFi
  
Other data warehouses:
* Redshift
* BigQuery
  
Other transformation alternatives:
* Spark
  
Potential changes to our solution:
* Airflow with dbt orchestration

## Built With
- Airflow, dbt, Snowflake and Postgres

## Authors
- Lucas Daudt - [Portfolio](lucasdaudt.com)
