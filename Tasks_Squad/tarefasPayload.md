**Nome do Estagiário:** Pablo Vinícius Domingues Sanches  
**Data:** 26/09/2024

# **Tarefas Geração de modelo para criação automatizada de views a partir de um payload Json**

### **Google Cloud e ambiente Local**
Utilizei o SDK do Google Cloud para facilitar a interação com a nuvem e conectei tudo ao VSCode. Isso tornou o desenvolvimento muito mais fluido. Apesar de alguns desafios com redes e permissões, cada etapa foi realizada com êxito.

## **Prompt, Otmização, Requisição e Validação**

``` python
# Importa o cliente do BigQuery e o módulo aiplatform do GCP
from google.cloud import bigquery
from google.cloud import aiplatform
from vertexai.preview.generative_models import GenerativeModel # --> Importa o modelo generativo do Vertex AI (neste caso, Gemini 1.5 Flash)
import json # --> Importa a biblioteca JSON para manipulação dos dados no formato JSON

# Inicializa o ambiente do Vertex AI no projeto 'squadcalouros'
aiplatform.init(project='squadcalouros')

# Carrega o modelo generativo Gemini
model = GenerativeModel("gemini-1.5-flash-002")

# Configura o cliente do BigQuery para realizar consultas no banco de dados
bq_client = bigquery.Client()

# Escreve a query SQL para buscar os campos 'data' e 'attributes' da tabela 'squadcalouros.dataform.eventos'
bq_query = """
SELECT 
    data,                
    attributes AS events 
FROM 
    squadcalouros.dataform.eventos;
"""

# Executa a consulta no BigQuery e armazena o resultado no objeto query_job
query_job = bq_client.query(bq_query)

# Transforma o resultado da consulta em uma lista de dicionários, onde cada dicionário contém as chaves 'data' e 'events'
query_data_str = json.dumps([{"data": row.data, "events": row.events} for row in query_job.result()])


# Esse prompt pede a geração de um script SQL que cria uma VIEW baseada nos dados JSON fornecidos
prompt = f"""Gere um script SQL que cria uma VIEW com base nas colunas do data JSON fornecido. 
Utilize 'CREATE OR REPLACE VIEW' e certifique-se de que as funções e tipos de dados sejam compatíveis com o BigQuery no GCP. 
Use a função 'JSON_VALUE' para extrair valores simples do JSON e o 'SAFE_CAST' para garantir que os dados sejam convertidos corretamente, 
lidando com valores nulos. O nome da VIEW será 'squadcalouros.dataform.event'. 
O script SQL deve incluir conversões de tipo apropriadas como 'STRING', 'TIMESTAMP' e 'BIGNUMERIC'. 
Certifique-se também de lidar corretamente com arrays, se existirem, utilizando a função 'UNNEST'. 
Aqui está o data JSON: {query_data_str}.

A tabela de origem é 'squadcalouros.dataform.eventos' e o caminho base é:
FROM
    squadcalouros.dataform.eventos AS t;
"""



# Envia o prompt para o modelo generativo, que retorna um script SQL
response = model.generate_content(prompt)

# Exibe o script SQL gerado pelo modelo
print(response)

```

## Query gerada + tratada:
```sql
query = 
CREATE OR REPLACE VIEW `squadcalouros.dataform.event` AS
SELECT
    SAFE_CAST(JSON_VALUE(data, '$.data.account.account_number') AS STRING) AS account_number,
    SAFE_CAST(JSON_VALUE(data, '$.data.account.account_type') AS STRING) AS account_type,
    SAFE_CAST(JSON_VALUE(data,   '$.data.account.bank.address.city') AS STRING) AS account_bank_city,
    SAFE_CAST(JSON_VALUE(data, '$.data.account.bank.address.state') AS STRING) AS account_bank_state,
    SAFE_CAST(JSON_VALUE(data, '$.data.account.bank.address.street') AS STRING) AS account_bank_street,
    SAFE_CAST(JSON_VALUE(data, '$.data.account.bank.address.zip_code') AS STRING) AS account_bank_zip_code,
    SAFE_CAST(JSON_VALUE(data, '$.data.account.bank.branch') AS STRING) AS account_bank_branch,
    SAFE_CAST(JSON_VALUE(data, '$.data.account.bank.name') AS STRING) AS account_bank_name,
    SAFE_CAST(JSON_VALUE(data, '$.data.account.currency') AS STRING) AS account_currency,
    SAFE_CAST(JSON_VALUE(data, '$.data.recipient.account_number') AS STRING) AS recipient_account_number,
    SAFE_CAST(JSON_VALUE(data, '$.data.recipient.bank.address.city') AS STRING) AS recipient_bank_city,
    SAFE_CAST(JSON_VALUE(data, '$.data.recipient.bank.address.state') AS STRING) AS recipient_bank_state,
    SAFE_CAST(JSON_VALUE(data, '$.data.recipient.bank.address.street') AS STRING) AS recipient_bank_street,
    SAFE_CAST(JSON_VALUE(data, '$.data.recipient.bank.address.zip_code') AS STRING) AS recipient_bank_zip_code,
    SAFE_CAST(JSON_VALUE(data, '$.data.recipient.bank.branch') AS STRING) AS recipient_bank_branch,
    SAFE_CAST(JSON_VALUE(data, '$.data.recipient.bank.name') AS STRING) AS recipient_bank_name,
    SAFE_CAST(JSON_VALUE(data, '$.data.recipient.name') AS STRING) AS recipient_name,
    SAFE_CAST(JSON_VALUE(data, '$.data.sender.account_number') AS STRING) AS sender_account_number,
    SAFE_CAST(JSON_VALUE(data, '$.data.sender.bank.address.city') AS STRING) AS sender_bank_city,
    SAFE_CAST(JSON_VALUE(data, '$.data.sender.bank.address.state') AS STRING) AS sender_bank_state,
    SAFE_CAST(JSON_VALUE(data, '$.data.sender.bank.address.street') AS STRING) AS sender_bank_street,
    SAFE_CAST(JSON_VALUE(data, '$.data.sender.bank.address.zip_code') AS STRING) AS sender_bank_zip_code,
    SAFE_CAST(JSON_VALUE(data, '$.data.sender.bank.branch') AS STRING) AS sender_bank_branch,
    SAFE_CAST(JSON_VALUE(data, '$.data.sender.bank.name') AS STRING) AS sender_bank_name,
    SAFE_CAST(JSON_VALUE(data, '$.data.sender.name') AS STRING) AS sender_name,
    SAFE_CAST(JSON_VALUE(data, '$.data.transaction_details.amount') AS BIGNUMERIC) AS transaction_amount,
    SAFE_CAST(JSON_VALUE(data, '$.data.transaction_details.description') AS STRING) AS transaction_description,
    SAFE_CAST(JSON_VALUE(data, '$.data.transaction_details.fees.amount') AS BIGNUMERIC) AS transaction_fees_amount,
    SAFE_CAST(JSON_VALUE(data, '$.data.transaction_details.fees.currency') AS STRING) AS transaction_fees_currency,
    SAFE_CAST(JSON_VALUE(data, '$.data.transaction_details.method') AS STRING) AS transaction_method,
    SAFE_CAST(JSON_VALUE(data, '$.data.transaction_details.reference_number') AS STRING) AS transaction_reference_number,
    SAFE_CAST(JSON_VALUE(data, '$.data.transaction_details.status') AS STRING) AS transaction_status,
    SAFE_CAST(JSON_VALUE(data, '$.data.transaction_details.timestamp') AS TIMESTAMP) AS transaction_timestamp,
    SAFE_CAST(JSON_VALUE(data, '$.data.transaction_details.type') AS STRING) AS transaction_type,
    SAFE_CAST(JSON_VALUE(data, '$.data.transaction_id') AS STRING) AS transaction_id
FROM
    `squadcalouros.dataform.eventos` AS t;
```

<br>

## **Criação de Arquivo e Upload para BigQuery**
 ```python 
 from google.cloud import bigquery

# Configuração do cliente BigQuery
client = bigquery.Client()

# Sua query para criar a view
query = """
CREATE OR REPLACE VIEW `squadcalouros.dataform.event1` AS
SELECT
    SAFE_CAST(JSON_VALUE(t.data, '$.account.account_number') AS STRING) AS account_account_number,
    SAFE_CAST(JSON_VALUE(t.data, '$.account.account_type') AS STRING) AS account_account_type,
    SAFE_CAST(JSON_VALUE(t.data, '$.account.bank.address.city') AS STRING) AS account_bank_address_city,
    SAFE_CAST(JSON_VALUE(t.data, '$.account.bank.address.state') AS STRING) AS account_bank_address_state,
    SAFE_CAST(JSON_VALUE(t.data, '$.account.bank.address.street') AS STRING) AS account_bank_address_street,
    SAFE_CAST(JSON_VALUE(t.data, '$.account.bank.address.zip_code') AS STRING) AS account_bank_address_zip_code,
    SAFE_CAST(JSON_VALUE(t.data, '$.account.bank.branch') AS STRING) AS account_bank_branch,
    SAFE_CAST(JSON_VALUE(t.data, '$.account.bank.name') AS STRING) AS account_bank_name,
    SAFE_CAST(JSON_VALUE(t.data, '$.account.currency') AS STRING) AS account_currency,
    SAFE_CAST(JSON_VALUE(t.data, '$.recipient.account_number') AS STRING) AS recipient_account_number,
    SAFE_CAST(JSON_VALUE(t.data, '$.recipient.bank.address.city') AS STRING) AS recipient_bank_address_city,
    SAFE_CAST(JSON_VALUE(t.data, '$.recipient.bank.address.state') AS STRING) AS recipient_bank_address_state,
    SAFE_CAST(JSON_VALUE(t.data, '$.recipient.bank.address.street') AS STRING) AS recipient_bank_address_street,
    SAFE_CAST(JSON_VALUE(t.data, '$.recipient.bank.address.zip_code') AS STRING) AS recipient_bank_address_zip_code,
    SAFE_CAST(JSON_VALUE(t.data, '$.recipient.bank.branch') AS STRING) AS recipient_bank_branch,
    SAFE_CAST(JSON_VALUE(t.data, '$.recipient.bank.name') AS STRING) AS recipient_bank_name,
    SAFE_CAST(JSON_VALUE(t.data, '$.recipient.name') AS STRING) AS recipient_name,
    SAFE_CAST(JSON_VALUE(t.data, '$.sender.account_number') AS STRING) AS sender_account_number,
    SAFE_CAST(JSON_VALUE(t.data, '$.sender.bank.address.city') AS STRING) AS sender_bank_address_city,
    SAFE_CAST(JSON_VALUE(t.data, '$.sender.bank.address.state') AS STRING) AS sender_bank_address_state,
    SAFE_CAST(JSON_VALUE(t.data, '$.sender.bank.address.street') AS STRING) AS sender_bank_address_street,
    SAFE_CAST(JSON_VALUE(t.data, '$.sender.bank.address.zip_code') AS STRING) AS sender_bank_address_zip_code,
    SAFE_CAST(JSON_VALUE(t.data, '$.sender.bank.branch') AS STRING) AS sender_bank_branch,
    SAFE_CAST(JSON_VALUE(t.data, '$.sender.bank.name') AS STRING) AS sender_bank_name,
    SAFE_CAST(JSON_VALUE(t.data, '$.sender.name') AS STRING) AS sender_name,
    SAFE_CAST(JSON_VALUE(t.data, '$.transaction_details.amount') AS BIGNUMERIC) AS transaction_details_amount,
    SAFE_CAST(JSON_VALUE(t.data, '$.transaction_details.description') AS STRING) AS transaction_details_description,
    SAFE_CAST(JSON_VALUE(t.data, '$.transaction_details.fees.amount') AS BIGNUMERIC) AS transaction_details_fees_amount,
    SAFE_CAST(JSON_VALUE(t.data, '$.transaction_details.fees.currency') AS STRING) AS transaction_details_fees_currency,
    SAFE_CAST(JSON_VALUE(t.data, '$.transaction_details.method') AS STRING) AS transaction_details_method,
    SAFE_CAST(JSON_VALUE(t.data, '$.transaction_details.reference_number') AS STRING) AS transaction_details_reference_number,
    SAFE_CAST(JSON_VALUE(t.data, '$.transaction_details.status') AS STRING) AS transaction_details_status,
    SAFE_CAST(JSON_VALUE(t.data, '$.transaction_details.timestamp') AS TIMESTAMP) AS transaction_details_timestamp,
    SAFE_CAST(JSON_VALUE(t.data, '$.transaction_details.type') AS STRING) AS transaction_details_type,
    SAFE_CAST(JSON_VALUE(t.data, '$.transaction_id') AS STRING) AS transaction_id,
    SAFE_CAST(JSON_VALUE(t.data, '$.events') AS STRING) AS events
FROM
    `squadcalouros.dataform.eventos` AS t;
"""

# Executar a query para criar a view
query_job = client.query(query)

# Aguardar a conclusão do job
query_job.result()

print("View criada com sucesso.")
 ```

