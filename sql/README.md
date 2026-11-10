# Validação SQL

## Objetivo

Antes de implementar a regra de cobertura de estoque no Qlik, a lógica foi validada no MySQL.

O modelo de demonstração utiliza três tabelas:

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

## Cálculo de cobertura

A métrica de negócio é:

```text
Cobertura de Estoque (dias) = Estoque Atual / Média Diária de Vendas
```

Uma consulta representativa de validação é:

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

## Classificação de negócio

Os limites utilizados na demonstração são:

- **Saudável**: cobertura > 7 dias
- **Atenção**: cobertura > 3 e <= 7 dias
- **Crítico**: cobertura <= 3 dias

Os limites são regras de portfólio e podem ser parametrizados de acordo com a política de reposição de uma empresa real.

## Por que validar primeiro no SQL?

Validar o cálculo no MySQL antes de reproduzi-lo no Qlik ajuda a confirmar que:

- os relacionamentos entre as tabelas estão corretos;
- a média diária de vendas está sendo calculada como esperado;
- a cobertura de estoque é matematicamente consistente;
- a classificação de risco se baseia em uma métrica validada, e não apenas em uma expressão visual.
