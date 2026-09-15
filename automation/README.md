# Qlik Automation

## Purpose

The automation layer turns a critical inventory condition into an operational action.

The project intentionally treats **Attention** and **Critical** products differently:

- **Attention** products remain available for analyst follow-up;
- **Critical** products can trigger an automated purchase-request flow.

## Workflow

```text
Critical product identified
        |
        v
Qlik Automation
        |
        v
Select relevant field values
        |
        v
Create bookmark / context
        |
        v
Retrieve table data
        |
        v
Build HTML message
        |
        v
Send e-mail
        |
        v
Buyer
```

## Data included in the e-mail

The generated purchase request includes:

- Product ID
- Product name
- Category
- Brand
- Current stock
- Average daily sales
- Coverage in days
- Product status

Critical products are consolidated into one communication rather than generating one separate e-mail per item.

## HTML template

The e-mail uses a custom HTML/CSS layout aligned with the StockGuard visual identity.

See [`purchase-request-email.html`](purchase-request-email.html).

## Evidence

The public portfolio includes screenshots of:

- the Qlik Automation workflow;
- the resulting purchase-request e-mail.

These screenshots use demonstration data only.
