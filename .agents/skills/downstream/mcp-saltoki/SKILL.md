---
name: mcp-saltoki
description: Guía operativa oficial del MCP downstream de Saltoki Online (búsqueda con parámetros category_id y category_slug, precios B2B y trazabilidad).
---

# Skill MCP Saltoki Online

## Directrices de Búsqueda en Catálogo
Para consultar productos específicos de aparellaje o material eléctrico, la herramienta `saltoki__search_saltoki_products` requiere especificar obligatoriamente ambos parámetros de categoría cuando se busca por familia:
- `category_id`: ID numérico (ej. `4826` para Protección residencial modular, `3591` para Electricidad).
- `category_slug`: Nombre formateado de la categoría (ej. `electricidad`).

## Herramientas Clave
- `saltoki__search_saltoki_products`: Búsqueda de productos con filtro por categoría.
- `saltoki__get_saltoki_product`: Consulta rápida de coste neto B2B y PVP de tarifa.
- `saltoki__trace_saltoki_document`: Trazabilidad completa Factura <-> Albarán <-> Pedido <-> SKU.
