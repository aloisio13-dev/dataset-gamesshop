# Características

- A empresa foca apenas em fabricação de consoles, deixando a distribuição e venda para terceiros
- Os produtos são vendidos globalmente

## Objetivos

- [x] Consolidar todas as bases de terceiros para realizar uma análise
- [x] Transformar dados de vendasem informações relevantes para a fabricante
- [x] Quais são os produtos mais populares em cada país
- [x] Como otimizar o processo de transporte e logística até o momento da venda

## Documentação do Banco de Dados - Meganium Sales Data

### Introdução

Este documento descreve a estrutura do banco de dados utilizado para armazenar dados de vendas de produtos Meganium no AliExpress. Inclui informações sobre produtos vendidos, pedidos realizados, detalhes dos compradores e descontos aplicados.

## Estrutura do Banco de Dados

### 1. Tabelas

#### **Tabela: produtos**

Contém informações sobre os produtos vendidos.

- `SKU` (VARCHAR, PRIMARY KEY) – Identificador único do produto.
- `nome_produto` (VARCHAR) – Nome do produto.
- `preco_unitario` (DECIMAL) – Preço unitário do produto.
- `site` (VARCHAR) – Plataforma onde o produto foi vendido.

#### **Tabela: pedidos**

Armazena dados sobre os pedidos realizados.

- `invoice_id` (UUID, PRIMARY KEY) – Identificador único da fatura do pedido.
- `SKU` (VARCHAR, FOREIGN KEY) – Referência ao produto comprado.
- `data_pedido` (DATE) – Data em que o pedido foi realizado.
- `quantidade` (INT) – Número de unidades compradas.
- `valor_total` (DECIMAL) – Valor total do pedido.
- `moeda` (VARCHAR) – Moeda da transação.
- `discount_coupon` (VARCHAR) – Código do cupom de desconto utilizado.
- `discount_value` (DECIMAL) – Valor do desconto aplicado.

## **Tabela: compradores**

Armazena dados sobre os clientes que realizaram compras.

- `buyer_id` (UUID, PRIMARY KEY) – Identificador único do comprador.
- `buyer_name` (VARCHAR) – Nome do comprador.
- `buyer_birth_date` (DATE) – Data de nascimento do comprador.
- `delivery_country` (VARCHAR) – País de entrega do pedido.

## 2. Relacionamentos

- **produtos ↔ pedidos**: Um produto pode estar em vários pedidos, mas cada pedido contém um único produto (referenciado pelo `SKU`).
- **compradores ↔ pedidos**: Um comprador pode realizar múltiplos pedidos, mas cada pedido pertence a um único comprador.

h1- Como Utilizar o Banco de Dados

Abaixo estão algumas consultas SQL úteis para interagir com o banco de dados:

### Recuperar todas as vendas de um determinado produto

```sql
SELECT * FROM pedidos WHERE SKU = 'SKU-35XX01';
SELECT compradores.buyer_name, pedidos.discount_coupon, pedidos.discount_value 
FROM compradores
JOIN pedidos ON compradores.buyer_id = pedidos.invoice_id
WHERE pedidos.discount_value > 0;
