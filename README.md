# Arquitetura de Coleta de Dados - UNIPÊ

Projeto da disciplina de Arquitetura de Coleta de Dados.
Pipeline em camadas Bronze -> Silver -> Gold usando Spark, MinIO, Iceberg e Postgres.

## Arquitetura

CSV (lote1 / lote2) -> Bronze (Iceberg) -> Silver (Iceberg) -> Gold
Gold -> Postgres (schema gold)

notebooks/spark-warehouse/
  01_bronze.ipynb - Ingestão dos CSVs para Bronze
  02_silver.ipynb - Limpeza e padronização para Silver
  03_gold.ipynb - Agregações para Gold
common/
  lake_local.py - Conexão Spark, MinIO e Postgres
  medalhao.py - Funções do medalhão
docker/
  docker-compose.yml - Postgres + MinIO
scripts/
  criar_buckets.py - Cria buckets no MinIO
  verifica_ambiente.py - Verifica se o ambiente está ok
data/
  gerar_dados.py - Gera os dados dos lotes


## Como rodar

### 1. Subir os serviços

cd docker
docker compose up -d

### 2. Criar os buckets

python scripts/criar_buckets.py

### 3. Rodar os notebooks em ordem

1. 01_bronze.ipynb
2. 02_silver.ipynb
3. 03_gold.ipynb

## Resultado final

Tabelas publicadas no Postgres, schema gold:
- gold.vendas_diarias
- gold.clientes_360
- gold.avaliacoes_resumo

## Tecnologias

Python 3.11, PySpark 3.5.3, Apache Iceberg, MinIO, Postgres 16, Docker