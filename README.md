# ⛅ Pipeline ETL: Clima de São Paulo (Airflow & Docker)

![Python](https://img.shields.io/badge/Python-3.12-blue?style=for-the-badge&logo=python)
![Apache Airflow](https://img.shields.io/badge/Apache%20Airflow-017CEE?style=for-the-badge&logo=Apache%20Airflow&logoColor=white)
![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/postgresql-4169e1?style=for-the-badge&logo=postgresql&logoColor=white)
![Pandas](https://img.shields.io/badge/pandas-%23150458.svg?style=for-the-badge&logo=pandas&logoColor=white)

## 🎯 Sobre o Projeto
Este é um projeto ponta a ponta de Engenharia de Dados focado na construção de um **Pipeline ETL** autônomo. O objetivo é extrair dados meteorológicos em tempo real da cidade de São Paulo, transformar esses dados para um formato otimizado e carregá-los em um banco de dados relacional para análises futuras.

Todo o fluxo de orquestração foi construído utilizando **Apache Airflow** rodando em um ambiente conteinerizado com **Docker**, garantindo isolamento, escalabilidade e facilidade de implantação.
## 🛠️ Tecnologias Utilizadas
* **Linguagem:** Python 3
* **Orquestração:** Apache Airflow
* **Contêinerização:** Docker & Docker Compose
* **Banco de Dados:** PostgreSQL 18
* **Processamento de Dados:** Pandas & Arrow (Formato Parquet)
* **Gerenciamento de Dependências:** `uv`
* **Interação com Banco:** SQLAlchemy & Psycopg2

## 🏗️ Arquitetura do Pipeline

<img width="1597" height="372" alt="Esquema" src="https://github.com/user-attachments/assets/57b5aef9-2950-49b9-b996-da478b0d3148" />


A DAG (`weather_pipeline`) foi estruturada em três tarefas principais sequenciais:

1. **Extract (`extract_data.py`):** Consome a API REST pública do OpenWeatherMap, validando a conexão e extraindo o JSON com as condições climáticas atuais de São Paulo.
2. **Transform (`transform_data.py`):** Utiliza `pandas` para achatar o JSON, limpar valores nulos, converter tipos de dados (timestamps) e padronizar colunas. O resultado é salvo temporariamente no formato **.parquet** para garantir alta compressão e tipagem forte durante a transição.
3. **Load (`load_data.py`):** Estabelece conexão com o PostgreSQL na máquina host utilizando `SQLAlchemy` e `psycopg2`, realizando a ingestão incremental (`append`) dos dados na tabela `sp_weather`.

## 📂 Estrutura do Repositório

```text
📦 tutorial_pipeline_weather
 ┣ 📂 assets                     # Imagens e documentação visual
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
```

## 📡 Origem dos Dados (Data Source)

Os dados processados neste pipeline são extraídos da **API REST pública do OpenWeatherMap**, especificamente do endpoint de *Current Weather Data*. 

* **Coleta:** O Airflow está agendado para fazer requisições automáticas a cada hora (`schedule='0 */1 * * *'`), garantindo uma granularidade fina do clima ao longo do dia.
* **Formato Bruto:** Os dados chegam no formato JSON, contendo informações meteorológicas aninhadas em tempo real da cidade de São Paulo (Latitude e Longitude específicas).
* **Atributos Extraídos:** O script foca em capturar as métricas mais relevantes para análise climática, como:
  * Temperatura atual, máxima e mínima (em graus Celsius)
  * Sensação térmica (*feels like*)
  * Percentual de umidade do ar
  * Condição climática geral (ex: *Clear*, *Rain*, *Clouds*)
  * Velocidade do vento

 ## 👁️ Visão do Airflow em Execução
<img width="1918" height="941" alt="Img projeto 2" src="https://github.com/user-attachments/assets/065a13b0-7fd6-4ad8-941a-9f582310fc7c" />

## 🗄️ Tabela do Banco de Dados (pgAdmin)
<img width="1917" height="1027" alt="Img projeto 1" src="https://github.com/user-attachments/assets/250e3666-1050-4f51-9c0f-0224720b74a9" />

## 📊 Resultados e Insights (Analytics)

A principal entrega técnica deste projeto é a construção de uma **infraestrutura resiliente e automatizada**. No entanto, com os dados devidamente tratados em formato `.parquet` e consolidados em uma tabela do **PostgreSQL**, abrimos portas para análises valiosas:

1. **Monitoramento Histórico e Tendências:** Diferente da API gratuita que mostra apenas o clima no momento da requisição, nosso banco de dados agora constrói um histórico contínuo (série temporal). Isso permite analisar a amplitude térmica de São Paulo ao longo das semanas ou meses.
2. **Correlações Climáticas:** Com os dados estruturados, é possível cruzar informações utilizando consultas SQL (ex: *Como a velocidade do vento afeta a sensação térmica em dias com alta umidade?*).
3. **Prontidão para Dashboards:** A tabela `sp_weather` gerada no final do pipeline está pronta para ser conectada a ferramentas de visualização de dados (como Power BI, Tableau ou Metabase). Isso permite a criação de painéis interativos mostrando picos de temperatura e frequência de chuvas na capital paulista.
4. **Alerta e Prevenção:** Em um cenário de negócios real (como varejo ou logística), essa base histórica poderia ser usada para prever demanda (ex: aumento de vendas de guarda-chuvas ou bebidas quentes) ou planejar rotas de entrega baseadas em padrões de chuva.
