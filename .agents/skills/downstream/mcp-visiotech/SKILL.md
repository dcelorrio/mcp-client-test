---
name: mcp-visiotech
description: Guía operativa del MCP Visiotech Security para catálogo, precios, stock, pedidos y fichas técnicas.
---

# Visiotech MCP Skill

Guía operativa para consultas al catálogo y servicios de Visiotech Security a través de MCP Gateway.

## Herramientas Principales
- `visiotech__search_visiotech_products`: Búsqueda de productos en catálogo Algolia (`query`, `page`, `limit`).
- `visiotech__get_visiotech_product`: Consulta detalle, precios (instalador y PVP), stock y especificaciones (`sku`).
- `visiotech__list_visiotech_invoices`: Lista facturas reales emitidas (`two_factor_code` opcional).
- `visiotech__list_visiotech_orders`: Lista historial de pedidos tramitados (`two_factor_code` opcional).
- `visiotech__get_visiotech_order_detail`: Desglose detallado de pedido (`order_id`, `two_factor_code` opcional).
- `visiotech__get_visiotech_account_profile`: Datos comerciales y cuenta (`two_factor_code` opcional).
- `visiotech__download_visiotech_datasheet`: Descarga PDF de ficha técnica (`sku`).
- `visiotech__get_visiotech_skill`: Devuelve guía operativa oficial.

## Reglas de Datos Obligatorios
1. Precio Neto Instalador (coste unitario).
2. PVP oficial.
3. Especificaciones clave y stock.
4. Enlace URL / Ficha técnica si aplica.
