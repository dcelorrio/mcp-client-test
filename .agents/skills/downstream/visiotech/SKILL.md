---
name: visiotech
description: Guía operativa del MCP Visiotech Security para catálogo, precios, stock y fichas técnicas.
---

# Visiotech MCP Skill

Guía operativa para consultas al catálogo de Visiotech Security a través de MCP Gateway.

## Herramientas Principales
- `visiotech__search_visiotech_products`: Búsqueda de productos en catálogo Algolia.
  - Parámetros: `query` (string), `page` (int, default 0), `limit` (int, default 20).
- `visiotech__get_visiotech_product`: Consulta detalle, precios (instalador y PVP), stock y especificaciones.
  - Parámetros: `sku` (string, obligatorio).
- `visiotech__list_visiotech_invoices`: Lista facturas reales emitidas.
- `visiotech__list_visiotech_orders`: Lista historial de pedidos tramitados.
- `visiotech__get_visiotech_order_detail`: Desglose detallado de pedido.
- `visiotech__get_visiotech_account_profile`: Datos comerciales y cuenta.
- `visiotech__download_visiotech_datasheet`: Descarga PDF de ficha técnica.
- `visiotech__get_visiotech_skill`: Devuelve guía operativa oficial.

## Reglas de Datos Obligatorios
1. Precio Neto Instalador (coste unitario).
2. PVP oficial.
3. Especificaciones clave.
4. Enlace URL / Ficha técnica.
