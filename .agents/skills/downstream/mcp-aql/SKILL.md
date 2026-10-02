---
name: mcp-aql
description: Habilidad operativa y de consulta para interactuar con la API REST y herramientas MCP de AQL Protección.
---

# AQL Protección MCP Assistant

Esta habilidad proporciona a los agentes la capacidad de interactuar con el portal B2B de **AQL Protección**.

## Herramientas Disponibles
- `aql__search_aql_products(query, page)`: Busca productos en catálogo B2B.
- `aql__list_aql_orders(page)`: Listado de pedidos con número, fecha, importe total y estado.
- `aql__get_aql_order_detail(order_id)`: Desglose completo de líneas de un pedido.

## Nota de Facturación
El portal web de AQL tiene las facturas desactivadas en tienda (`PS_INVOICE = 0`), gestionándose administrativamente. El seguimiento B2B se realiza mediante pedidos de compra.
