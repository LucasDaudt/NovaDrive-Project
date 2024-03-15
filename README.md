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

