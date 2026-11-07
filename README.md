# Arquitetura de Coleta de Dados - UNIPE

Projeto da disciplina de Arquitetura de Coleta de Dados - Pos-graduacao UNIPE
Aluno: Alexandre S. M. | Professor: Wuldson Franco

Pipeline em camadas Bronze -> Silver -> Gold usando Apache Spark, Apache Iceberg, MinIO e PostgreSQL.

## Arquitetura

CSV (lote1 / lote2) -> Bronze (Iceberg) -> Silver (Iceberg) -> Gold (Iceberg) -> PostgreSQL (schema gold)

- MinIO: Armazenamento de objetos (Data Lake)
- Spark: Processamento distribuido
- PostgreSQL: Banco final com as tabelas Gold

## Estrutura do Projeto

- common/lake_local.py - Conexao Spark, MinIO e Postgres
- common/medallion.py - Funcoes do medalhao
- docker/docker-compose.yml - Sobe Spark, MinIO e Postgres
- notebooks/spark-warehouse/01_bronze.ipynb - Ingestao dos CSVs para Bronze
- notebooks/spark-warehouse/02_silver.ipynb - Limpeza e padronizacao para Silver
- notebooks/spark-warehouse/03_gold.ipynb - Agregacoes para Gold
- notebooks_pandas/ - Versao alternativa com Pandas
- scripts/criar_buckets.py - Cria buckets no MinIO
- scripts/verifica_ambiente.py - Verifica se o ambiente esta ok
- scripts/gerar_dados.py - Gera os dados dos lotes
- data/ - CSVs de entrada (lote1 e lote2)

## Como Rodar

### 1. Subir os servicos
cd docker
docker compose up -d

### 2. Criar os buckets no MinIO
python scripts/criar_buckets.py

### 3. Verificar se esta tudo ok
python scripts/verifica_ambiente.py

### 4. Rodar os notebooks em ordem
1. notebooks/spark-warehouse/01_bronze.ipynb
2. notebooks/spark-warehouse/02_silver.ipynb
3. notebooks/spark-warehouse/03_gold.ipynb

## Resultado Final

Tabelas publicadas no PostgreSQL, no schema gold:

- gold.vendas_diarias - Total de vendas por dia
- gold.clientes_360 - Visao 360 de cada cliente
- gold.avaliacoes_resumo - Resumo das avaliacoes dos produtos

## Evidências e Entregas

Arquivos de entrega das Partes 2 e 3:

- [Evidências - Projeto Final](./Evidências%20-%20Projeto%20Final.xlsx)
- [Respostas - Projeto Final](./Respostas%20-%20Projeto%20Final.txt)

## Tecnologias

Python 3.11, PySpark 3.5.3, Apache Iceberg, MinIO, PostgreSQL 16, Docker, Docker Compose, Jupyter Notebook