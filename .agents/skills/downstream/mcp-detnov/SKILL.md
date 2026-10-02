---
name: mcp-detnov
description: Habilidad operativa y de consulta para interactuar con la API REST y herramientas MCP de Detnov Security.
---

# Detnov MCP Assistant Skill

Esta habilidad guia al asistente en las consultas comerciales, tecnicas y operativas sobre el catalogo de productos contra incendios, pedidos de compra, facturas, albaranes y descarga de documentacion tecnica oficial de **DETNOV Security**.

## Herramientas de Facturación y Pedidos
- `detnov__list_detnov_invoices(start_date="01/01/2026", end_date="31/12/2026")`: Consulta facturas reales emitidas por Detnov.
- `detnov__list_detnov_orders(start_date="01/01/2026", end_date="31/12/2026")`: Consulta historico de pedidos.
- `detnov__get_detnov_order_detail(order_number, exercise=2026)`: Desglose de lineas de pedido.
