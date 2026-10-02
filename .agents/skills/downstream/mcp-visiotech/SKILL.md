---
name: mcp-visiotech
description: Habilidad operativa y de consulta para interactuar con la API REST y herramientas MCP de Visiotech Security.
---

# Visiotech MCP Assistant Skill

Esta habilidad guía al asistente en las consultas comerciales, técnicas y operativas sobre el catálogo de Visiotech Security.

## Herramientas Registradas
- `visiotech__search_visiotech_products(query, page, limit)`: Búsqueda rápida de productos en el motor Algolia oficial.
- `visiotech__get_visiotech_product(sku)`: Ficha técnica detallada, PVP oficial, precio neto de instalador y disponibilidad/stock.
- `visiotech__list_visiotech_invoices()`: Lista facturas reales emitidas.
- `visiotech__list_visiotech_orders()`: Historial de pedidos reales.
- `visiotech__get_visiotech_account_profile()`: Configuración comercial.
