# Menu Performance Financeira
📊 Data Pipeline & BI Analytics – Python, PostgreSQL e Power BI

Projeto end-to-end de dados, desenvolvido e estruturado para ambiente de produção, cobrindo ETL completo, modelagem de dados e relatórios analíticos em Power BI, com foco em governança, rastreabilidade e performance.

O projeto foi pensado para ser escalável, auditável e automatizável, exigindo apenas pequenas configurações para adaptação a diferentes ambientes.

🧱 Arquitetura Geral

Fluxo do projeto:

Arquivos Fonte (Excel / CSV)
        ↓
Python (ETL em Jupyter Notebook)
        ↓
PostgreSQL (Staging + Data Warehouse)
        ↓
Power BI (Modelo Snowflake + DAX)

🛠️ Ferramentas Utilizadas
🔹 Linguagens & Plataformas

Python (ETL)

PostgreSQL (Data Warehouse)

Power BI (Modelagem e Visualização)

🔹 Ambiente Python

Desenvolvimento realizado em Jupyter Notebook

Pronto para migração para scripts .py ou execução agendada

🔹 Bibliotecas Python

pandas

pathlib

datetime

uuid

sqlalchemy

urllib.parse

🔄 ETL – Processo Completo em Python

Todo o processo de Extração, Transformação e Carga (ETL) foi implementado integralmente em Python.

1️⃣ Extração

O script:

Lê um arquivo fonte com múltiplas abas

Cria um dicionário de DataFrames, onde cada aba representa uma tabela

Estrutura preparada para:

Execução automática

Integração com pastas em nuvem

Uso de listener de modificação de arquivos (já disponível pelo autor)

📌 Produção:
Basta alterar o caminho da pasta e definir se a execução ocorrerá:

a cada modificação do arquivo
ou

em intervalos de tempo fixos (ex.: diário, horário)

2️⃣ Transformação

Regras aplicadas de forma padronizada em todas as tabelas:

🔤 Campos de texto

Convertidos para minúsculo

Remoção de espaços em branco no início e fim

📅 Campos de data

Conversão para tipo date/datetime

Padronização de formato (dia, mês e ano corretamente ordenados)

Essas regras garantem:

Consistência

Redução de ruído

Facilidade de relacionamento no BI

3️⃣ Carregamento (Load)
🔹 Staging Layer

Os dados são carregados inicialmente em uma camada intermediária (staging), que funciona como um meio termo entre dados brutos e modelo analítico final.

Características:

Armazena:

Número do batch

Data e hora de carregamento

Permite:

Auditoria

Reprocessamento

Rastreabilidade completa dos dados

🔹 Data Warehouse (Fatos e Dimensões)

Após a validação na staging:

Os dados alimentam as tabelas Fato e Dimensão

Separação clara entre:

Fatos → eventos mensuráveis

Dimensões → atributos de classificação e filtros

🧩 Modelagem de Dados – Power BI

Modelo construído em Snowflake

Relacionamentos realizados via IDs

Uso de Dimensão de Tempo central, compartilhada entre todos os fatos

Modelo otimizado para:

Performance

Clareza analítica

Escalabilidade

📐 Medidas DAX Implementadas
🔹 Inadimplência
Inadimplência Valor = 
CALCULATE (
    SUM ( 'dw fato_contas_receber'[valor] ),
    'dw dim_status'[status] = "atrasado"
)

Inadimplência % = 
DIVIDE (
    [Inadimplência Valor],
    SUM ( 'dw fato_contas_receber'[valor] )
)

🔹 Custos e Despesas
Custo Total = 
CALCULATE (
    SUM ( 'dw fato_custos_despesas'[valor] ),
    'dw dim_centro_custo'[centro_custo] = "operacional"
)

Despesa Total = 
CALCULATE (
    SUM ( 'dw fato_custos_despesas'[valor] ),
    'dw dim_centro_custo'[centro_custo] IN {"administrativo", "comercial"}
)

Custo sobre Receita % = 
DIVIDE ( [Custo Total], [Receita Bruta] )

Despesa sobre Receita % = 
DIVIDE ( [Despesa Total], [Receita Bruta] )

🔹 Receita e Margens
Receita Bruta = 
SUM ('dw fato_faturamento'[receita_bruta])

Receita Liquida = 
SUM ('dw fato_faturamento'[receita_liquida])

Margem Bruta = 
[Receita Bruta] - [Custo Total]

Margem Bruta % = 
DIVIDE ( [Margem Bruta], [Receita Bruta] )

EBITDA = 
[Receita Liquida] - [Custo Total] - [Despesa Total]

Margem Líquida % = 
DIVIDE (
    [Receita Liquida] - [Custo Total] - [Despesa Total],
    [Receita Liquida]
)

🔹 Análises Temporais
Receita MoM % = 
VAR Atual = [Receita Liquida]
VAR Anterior =
    CALCULATE (
        [Receita Liquida],
        DATEADD ('dw dim_tempo'[data], -1, MONTH)
    )
RETURN
DIVIDE (Atual - Anterior, Anterior)

Receita YoY % = 
VAR Atual = [Receita Liquida]
VAR AnoAnterior =
    CALCULATE (
        [Receita Liquida],
        SAMEPERIODLASTYEAR ( 'dw dim_tempo'[data] )
    )
RETURN
DIVIDE ( Atual - AnoAnterior, AnoAnterior )

🔹 Indicadores de Eficiência
Ticket Médio = 
DIVIDE (
    [Receita Bruta],
    DISTINCTCOUNT ( 'dw dim_cliente'[cliente_id] )
)

🔹 Orçamento
Valor Orçado Despesa = 
CALCULATE (
    SUM ( 'dw fato_orcamento'[valor_orcado] ),
    'dw dim_tipo_orcamento'[tipo] = "despesa"
)

Valor Orçado Receita = 
CALCULATE (
    SUM ( 'dw fato_orcamento'[valor_orcado] ),
    'dw dim_tipo_orcamento'[tipo] = "receita"
)

Aderência Orçamentária Despesa % = 
DIVIDE (
    SUM('dw fato_custos_despesas'[valor]),
    [Valor Orçado Despesa]
)

Aderência Orçamentária Receita % = 
DIVIDE (
    [Receita Liquida],
    [Valor Orçado Receita]
)
