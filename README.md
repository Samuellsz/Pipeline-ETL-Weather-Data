# ⛅ Weather ETL Pipeline - São Paulo

## 🎯 Sobre o Projeto
Este projeto é um pipeline de Engenharia de Dados ponta a ponta (ETL) criado para extrair, transformar e carregar dados climáticos da cidade de São Paulo. Todo o fluxo é automatizado e orquestrado usando **Apache Airflow** rodando em contêineres **Docker**, com os dados sendo finalmente armazenados em um banco de dados **PostgreSQL** local.

## 🛠️ Tecnologias Utilizadas
* **Linguagem:** Python 3
* **Orquestração:** Apache Airflow
* **Contêinerização:** Docker & Docker Compose
* **Banco de Dados:** PostgreSQL 16
* **Processamento de Dados:** Pandas & Arrow (Formato Parquet)
* **Gerenciamento de Dependências:** `uv`
* **Interação com Banco:** SQLAlchemy & Psycopg2

## 🏗️ Arquitetura do Pipeline

O pipeline segue a estrutura clássica de ETL, dividido em tarefas independentes dentro de uma DAG (`weather_pipeline`) no Airflow:

1. **Extract (`extract_data.py`):** Consome a API pública do OpenWeather para coletar os dados climáticos atuais (temperatura, umidade, condições) da cidade de São Paulo.
2. **Transform (`transform_data.py`):** Utiliza a biblioteca Pandas para limpar, padronizar e tratar os dados brutos recebidos da API. Os dados tratados são salvos temporariamente em formato **.parquet** (para maior eficiência e compressão) dentro do contêiner.
3. **Load (`load_data.py`):** Lê o arquivo Parquet e realiza a ingestão (append) dos dados estruturados em uma tabela no PostgreSQL local, garantindo que o histórico climático seja construído de forma contínua.

## ⚙️ Pré-requisitos
Para rodar este projeto na sua máquina, você precisará ter instalado:
* [Docker Desktop](https://www.docker.com/products/docker-desktop/)
* [PostgreSQL](https://www.postgresql.org/download/) (Rodando na porta 5432)
* Python 3.12+ 

## 🚀 Como Executar o Projeto

**1. Clone o repositório**
```bash
git clone [https://github.com/SEU_USUARIO/NOME_DO_REPOSITORIO.git](https://github.com/SEU_USUARIO/NOME_DO_REPOSITORIO.git)
cd NOME_DO_REPOSITORIO
