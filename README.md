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

| Medida | Código DAX |
|------|-----------|
| **Inadimplência Valor** | `CALCULATE (SUM ( 'dw fato_contas_receber'[valor] ),'dw dim_status'[status] = "atrasado")` |
| **Inadimplência %** | `DIVIDE ([Inadimplência Valor],SUM ( 'dw fato_contas_receber'[valor] ))` |
| **Custo Total** | `CALCULATE (SUM ( 'dw fato_custos_despesas'[valor] ),'dw dim_centro_custo'[centro_custo] = "operacional")` |
| **Despesa Total** | `CALCULATE (SUM ( 'dw fato_custos_despesas'[valor] ),'dw dim_centro_custo'[centro_custo] IN {"administrativo", "comercial"})` |
| **Custo sobre Receita %** | `DIVIDE ([Custo Total],[Receita Bruta])` |
| **Despesa sobre Receita %** | `DIVIDE ([Despesa Total],[Receita Bruta])` |
| **Receita Bruta** | `SUM ( 'dw fato_faturamento'[receita_bruta] )` |
| **Receita Líquida** | `SUM ( 'dw fato_faturamento'[receita_liquida] )` |
| **Margem Bruta** | `[Receita Bruta] - [Custo Total]` |
| **Margem Bruta %** | `DIVIDE ([Margem Bruta],[Receita Bruta])` |
| **EBITDA** | `[Receita Liquida] - [Custo Total] - [Despesa Total]` |
| **Margem Líquida %** | `DIVIDE ([Receita Liquida] - [Custo Total] - [Despesa Total],[Receita Liquida])` |
| **Receita MoM %** | `VAR Atual = [Receita Liquida] VAR Anterior = CALCULATE ( [Receita Liquida], DATEADD ( 'dw dim_tempo'[data], -1, MONTH ) ) RETURN DIVIDE ( Atual - Anterior, Anterior )` |
| **Receita YoY %** | `VAR Atual = [Receita Liquida] VAR AnoAnterior = CALCULATE ( [Receita Liquida], SAMEPERIODLASTYEAR ( 'dw dim_tempo'[data] ) )  RETURN DIVIDE ( Atual - AnoAnterior, AnoAnterior )` |
| **Ticket Médio** | `DIVIDE ( [Receita Bruta], DISTINCTCOUNT ( 'dw dim_cliente'[cliente_id] ))` |
| **Valor Orçado Despesa** | `CALCULATE ( SUM ( 'dw fato_orcamento'[valor_orcado] ), 'dw dim_tipo_orcamento'[tipo] = "despesa" )` |
| **Valor Orçado Receita** | `CALCULATE ( SUM ( 'dw fato_orcamento'[valor_orcado] ), 'dw dim_tipo_orcamento'[tipo] = "receita" )` |
| **Aderência Orçamentária Despesa %** | `DIVIDE ( SUM ( 'dw fato_custos_despesas'[valor] ), [Valor Orçado Despesa] )` |
| **Aderência Orçamentária Receita %** | `DIVIDE ( [Receita Liquida], [Valor Orçado Receita] )` |

