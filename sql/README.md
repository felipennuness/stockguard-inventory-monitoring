# SQL Validation

## Purpose

Before implementing the inventory-coverage rule in Qlik, the logic was validated in MySQL.

The demonstration model uses three tables:

```sql
CREATE TABLE produto (
    id_produto INT AUTO_INCREMENT PRIMARY KEY,
    nome VARCHAR(150) NOT NULL,
    categoria VARCHAR(100) NOT NULL,
    marca VARCHAR(100) NOT NULL
);
```

```sql
CREATE TABLE venda_diaria (
    id_venda INT AUTO_INCREMENT PRIMARY KEY,
    id_produto INT NOT NULL,
    data_venda DATE NOT NULL,
    quantidade_vendida INT NOT NULL,

    CONSTRAINT fk_venda_produto
        FOREIGN KEY (id_produto)
        REFERENCES produto(id_produto)
);
```

```sql
CREATE TABLE estoque (
    id_produto INT PRIMARY KEY,
    quantidade_atual INT NOT NULL,
    estoque_minimo INT NOT NULL,
    ultima_atualizacao DATETIME NOT NULL,

    CONSTRAINT fk_estoque_produto
        FOREIGN KEY (id_produto)
        REFERENCES produto(id_produto)
);
```

## Coverage calculation

The business metric is:

```text
Stock Coverage (days) = Current Stock / Average Daily Sales
```

A representative validation query is:

```sql
SELECT
    p.id_produto,
    p.nome,
    e.quantidade_atual,
    ROUND(AVG(v.quantidade_vendida), 2) AS media_venda_diaria,
    ROUND(
        e.quantidade_atual / AVG(v.quantidade_vendida),
        2
    ) AS cobertura_dias
FROM produto p
INNER JOIN estoque e
    ON p.id_produto = e.id_produto
INNER JOIN venda_diaria v
    ON p.id_produto = v.id_produto
GROUP BY
    p.id_produto,
    p.nome,
    e.quantidade_atual;
```

## Business classification

The demonstration thresholds are:

- **Healthy**: coverage > 7 days
- **Attention**: coverage > 3 and <= 7 days
- **Critical**: coverage <= 3 days

The thresholds are portfolio rules and can be parameterized according to a real company's replenishment policy.

## Why validate in SQL first?

Validating the calculation in MySQL before reproducing it in Qlik helps confirm that:

- relationships between the tables are correct;
- average daily sales are being calculated as expected;
- stock coverage is mathematically consistent;
- risk classification is based on a verified metric rather than only a visual expression.
