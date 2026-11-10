# Qlik Automation

## Objetivo

A camada de automação transforma uma condição crítica de estoque em uma ação operacional.

O projeto trata intencionalmente produtos em situação de **Atenção** e **Crítico** de maneiras diferentes:

- produtos em **Atenção** permanecem disponíveis para acompanhamento do analista;
- produtos **Críticos** podem acionar um fluxo automatizado de solicitação de compra.

## Fluxo de trabalho

```text
Produto crítico identificado
        |
        v
Qlik Automation
        |
        v
Selecionar valores de campos relevantes
        |
        v
Criar marcador / contexto
        |
        v
Recuperar dados da tabela
        |
        v
Montar mensagem HTML
        |
        v
Enviar e-mail
        |
        v
Comprador
```

## Dados incluídos no e-mail

A solicitação de compra gerada inclui:

- ID do produto
- Nome do produto
- Categoria
- Marca
- Estoque atual
- Média diária de vendas
- Cobertura em dias
- Status do produto

Os produtos críticos são consolidados em uma única comunicação, em vez de gerar um e-mail separado para cada item.

## Modelo HTML

O e-mail utiliza um layout personalizado em HTML / CSS, alinhado à identidade visual do StockGuard.

Consulte [purchase-request-email.html](purchase-request-email.html).

## Evidências

O portfólio público inclui capturas de tela de:

- fluxo do Qlik Automation;
- e-mail de solicitação de compra resultante.

Essas capturas utilizam apenas dados de demonstração.
