# Menu Performance Financeira

# 📊 Data Pipeline & Business Intelligence – Python, PostgreSQL e Power BI
Projeto **end-to-end de dados**, desenvolvido com foco em **ambiente de produção**, cobrindo todo o ciclo de **ETL**, modelagem de dados e visualização analítica em **Power BI**.
O projeto foi concebido para ser **escalável, auditável e automatizável**, exigindo apenas configurações mínimas para adaptação a novos ambientes ou fontes de dados.
---
## 🧱 Arquitetura do Projeto
Arquivos Fonte (Excel / CSV) -> Python (ETL – Jupyter Notebook) -> PostgreSQL (Staging + Data Warehouse) -> Power BI (Modelo Snowflake + DAX)
---
## 🛠️ Tecnologias Utilizadas
### Linguagens e Plataformas
- **Python**
- **PostgreSQL**
- **Power BI**
### Ambiente Python
- Desenvolvimento realizado em **Jupyter Notebook**
- Estrutura pronta para execução automatizada ou migração para scripts `.py`
### Bibliotecas Python
- `pandas`
- `pathlib`
- `datetime`
- `uuid`
- `sqlalchemy`
- `urllib.parse`
---
## 🔄 Processo ETL (Python)
Todo o processo de **Extração, Transformação e Carga (ETL)** foi implementado integralmente em Python.
### 1️⃣ Extração
- Leitura automática de arquivos com **múltiplas abas**
- Criação de um **dicionário de DataFrames**, um para cada aba
- Estrutura preparada para execução em rotina produtiva
📌 **Produção:**  
Basta definir o caminho da pasta (local ou nuvem) e configurar:
- Execução a cada modificação do arquivo  
ou  
- Execução em intervalos de tempo fixos
---
### 2️⃣ Transformação
Padronizações aplicadas a todas as tabelas:
- Campos de texto convertidos para **minúsculo**
- Remoção de espaços em branco no início e no fim
- Campos de data convertidos para tipo correto (`date/datetime`)
- Padronização de dia, mês e ano
---
### 3️⃣ Carregamento
#### Staging Layer
- Camada intermediária entre dados brutos e modelo analítico
- Armazena:
  - Número do **batch**
  - Data e hora do carregamento
- Permite:
  - Auditoria
  - Rastreabilidade
  - Reprocessamento
#### Data Warehouse
- Alimentação das **tabelas Fato e Dimensão**
- Separação clara entre:
  - **Fatos:** eventos mensuráveis
  - **Dimensões:** atributos de classificação
---
## 🧩 Modelagem de Dados (Power BI)
- Modelo em **Snowflake**
- Relacionamentos por **IDs**
- Dimensão de tempo central compartilhada entre os fatos
- Estrutura otimizada para performance e clareza analítica
<img width="1010" height="624" alt="image" src="https://github.com/user-attachments/assets/a1edc312-35ab-4405-b55c-c68633c5fdf6" />

<img width="982" height="580" alt="image" src="https://github.com/user-attachments/assets/fe2c6d76-e431-4658-bff7-1532fb746d5b" />
---
## 📐 Medidas DAX
| Categoria | Medida | Descrição |
|--------|--------|-----------|
| Inadimplência | **Inadimplência Valor** | Soma dos valores em atraso |
| Inadimplência | **Inadimplência %** | Percentual de inadimplência sobre o total |
| Custos | **Custo Total** | Soma dos custos operacionais |
| Custos | **Despesa Total** | Soma das despesas administrativas e comerciais |
| Custos | **Custo sobre Receita %** | Relação custo operacional / receita |
| Custos | **Despesa sobre Receita %** | Relação despesa / receita |
| Receita | **Receita Bruta** | Soma da receita bruta |
| Receita | **Receita Líquida** | Soma da receita líquida |
| Rentabilidade | **Margem Bruta** | Receita Bruta – Custo Total |
| Rentabilidade | **Margem Bruta %** | Margem bruta percentual |
| Rentabilidade | **EBITDA** | Resultado operacional |
| Rentabilidade | **Margem Líquida %** | Margem líquida percentual |
| Temporal | **Receita MoM %** | Crescimento mensal da receita |
| Temporal | **Receita YoY %** | Crescimento anual da receita |
| Eficiência | **Ticket Médio** | Receita por cliente |
| Orçamento | **Valor Orçado Receita** | Valor orçado para receita |
| Orçamento | **Valor Orçado Despesa** | Valor orçado para despesas |
| Orçamento | **Aderência Orçamentária Receita %** | Receita realizada / orçada |
| Orçamento | **Aderência Orçamentária Despesa %** | Despesa realizada / orçada |
---
