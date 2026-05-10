# ⛅ Pipeline ETL: Clima de São Paulo (Airflow & Docker)

![Python](https://img.shields.io/badge/Python-3.12-blue?style=for-the-badge&logo=python)
![Apache Airflow](https://img.shields.io/badge/Apache%20Airflow-017CEE?style=for-the-badge&logo=Apache%20Airflow&logoColor=white)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/postgresql-4169e1?style=for-the-badge&logo=postgresql&logoColor=white)
![Pandas](https://img.shields.io/badge/pandas-%23150458.svg?style=for-the-badge&logo=pandas&logoColor=white)

## 🎯 Sobre o Projeto
Este é um projeto ponta a ponta de Engenharia de Dados focado na construção de um **Pipeline ETL** autônomo. O objetivo é extrair dados meteorológicos em tempo real da cidade de São Paulo, transformar esses dados para um formato otimizado e carregá-los em um banco de dados relacional para análises futuras.

Todo o fluxo de orquestração foi construído utilizando **Apache Airflow** rodando em um ambiente conteinerizado com **Docker**, garantindo isolamento, escalabilidade e facilidade de implantação.

## 🏗️ Arquitetura do Pipeline

A DAG (`weather_pipeline`) foi estruturada em três tarefas principais sequenciais:

1. **Extract (`extract_data.py`):** Consome a API REST pública do OpenWeatherMap, validando a conexão e extraindo o JSON com as condições climáticas atuais de São Paulo.
2. **Transform (`transform_data.py`):** Utiliza `pandas` para achatar o JSON, limpar valores nulos, converter tipos de dados (timestamps) e padronizar colunas. O resultado é salvo temporariamente no formato **.parquet** para garantir alta compressão e tipagem forte durante a transição.
3. **Load (`load_data.py`):** Estabelece conexão com o PostgreSQL na máquina host utilizando `SQLAlchemy` e `psycopg2`, realizando a ingestão incremental (`append`) dos dados na tabela `sp_weather`.

## 📂 Estrutura do Repositório

```text
📦 tutorial_pipeline_weather
 ┣ 📂 dags
 ┃ ┗ 📜 weather_pipeline.py      # Definição da DAG do Airflow
 ┣ 📂 src
 ┃ ┣ 📜 extract_data.py          # Script de extração (API)
 ┃ ┣ 📜 transform_data.py        # Script de transformação (Pandas)
 ┃ ┗ 📜 load_data.py             # Script de carga (PostgreSQL)
 ┣ 📜 .env                       # Variáveis de ambiente (Ignorado no Git)
 ┣ 📜 .gitignore                 # Arquivos ignorados pelo controle de versão
 ┣ 📜 docker-compose.yaml        # Configuração dos contêineres do Airflow
 ┣ 📜 README.md                  # Documentação do projeto
 ┗ 📜 pyproject.toml             # Gerenciamento de dependências (uv)
