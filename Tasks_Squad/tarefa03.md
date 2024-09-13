**Nome do Estagiário:** Pablo Vinícius Domingues Sanches  
**Data:** 06/09/2024

# **Manipulação de Dados através do BigQuery com SQL**

### **Normalização dos dados**
- Removendo os dados repetidos.
- Combinando os dados das tabelas 'Campaign' e 'Purchase'.

```sql

CREATE OR REPLACE VIEW squadcalouros.squadcalouros.campaigns_purchases AS
SELECT DISTINCT
    int64_field_0 AS campaign_id,
    c.type_campaign,
    c.days_valid,
    c.data_campaign,
    c.channel,
    c.return_status,
    c.return_date,
    c.client_id,
    p.purchase_id,
    p.product_name,
    p.product_id,
    p.amount,
    p.price,
    p.discount_applied,
    p.payment_method,
    p.purchase_datetime,
    p.purchase_location
FROM 
    squadcalouros.squadcalouros.campaigns c
LEFT JOIN 
    squadcalouros.squadcalouros.purchases p
ON 
    c.client_id = p.client_id
```
<br>

- `CREATE OR REPLACE VIEW`: O comando **CREATE OR REPLACE VIEW** em SQL é utilizado para criar uma nova visão (view) ou substituir uma visão existente com o mesmo nome. 

<br>

### **View 1**
- Esta view agrupa as compras por cliente, calculando o total gasto e os locais de compra mais frequentes.

```sql

CREATE OR REPLACE VIEW `squadcalouros.squadcalouros.Vcompras_cliente` AS


WITH base AS ( --> Com a cláusula WITH, definimos subconsultas que serão usadas posteriormente

    
    SELECT --> Subconsulta base para calcular a contagem de compras por cliente e localização

        client_id,                       -- Seleciona o ID do cliente
        purchase_location,               -- Seleciona a localização da compra
        COUNT(*) AS location_count       -- Conta o número de compras por localização
    FROM
        `squadcalouros.squadcalouros.purchases`  
    GROUP BY
        client_id, purchase_location     -- Agrupa por cliente e localização de compra
),

most_frequent_location AS (
    
    SELECT --> Subconsulta para identificar a localização de compra mais frequente por cliente

        client_id,                                     -- Seleciona o ID do cliente
        purchase_location,                             -- Seleciona a localização da compra
        ROW_NUMBER() OVER (                            -- Atribui um número de linha para ordenar
            PARTITION BY client_id                     -- Particiona por cliente

            ORDER BY location_count DESC, purchase_location ASC  --> Ordena pela contagem de localização (desc) e localização (asc)

        ) AS row_num -- Número da linha, usado para identificar o local mais frequente
    FROM
        base --> Usa a subconsulta base
)


SELECT --> Seleciona e calcula as métricas finais para a VIEW

    p.client_id,                                        -- Seleciona o ID do cliente
    SUM(p.price * p.amount * (1 - COALESCE(p.discount_applied, 0))) AS total_price,  

    --> Calcula o preço total das compras por cliente, ajustando por qualquer desconto aplicado <--
    
    mfl.purchase_location AS most_purchase_location,   -- Seleciona a localização de compra mais frequente

    MIN(p.purchase_datetime) AS first_purchase,        -- Determina a data da primeira compra do cliente
    
    MAX(p.purchase_datetime) AS last_purchase,         -- Determina a data da última compra do cliente
    
    FORMAT_TIMESTAMP('%Y-%m-%d', CURRENT_TIMESTAMP()) AS date_today,  
    -- Adiciona a data atual formatada como 'YYYY-MM-DD'
    
    CAST(FORMAT_TIMESTAMP('%m%Y', CURRENT_TIMESTAMP()) AS INT64) AS anomes_today  
    -- Adiciona o ano e mês atual no formato 'MMYYYY' convertido para inteiro
FROM
    `squadcalouros.squadcalouros.purchases` p          
JOIN
    most_frequent_location mfl ON p.client_id = mfl.client_id 
    AND mfl.row_num = 1  --> Junta com a subconsulta para trazer apenas a localização mais frequente (row_num = 1)

GROUP BY
    p.client_id, mfl.purchase_location; --> Agrupa o resultado final por ID do cliente e localização de compra mais frequente
```

<br>

`WiTH...AS (Common Table Expressions - CTEs)`: é usado para definir Common Table Expressions (CTEs). CTEs são subconsultas nomeadas que existem apenas durante a execução da consulta principal e são úteis para organizar o código SQL, quebrar consultas complexas em partes menores e reutilizar resultados intermediários.
