---
name: mcp-ibd
description: Guía de uso y capacidades del MCP downstream IBD Global.
---

# Skill MCP IBD Global

## Descripción
Este MCP interactúa con el portal de IBD Global Spain para consultar catálogo de productos, pedidos, albaranes, facturas y datos de cuenta.

## Herramientas Registradas (8 tools)

### Productos
- `ibd__search_ibd_products`: Busca productos, referencias Dahua y precios de instalador B2B en el catálogo de IBD Global.

### Pedidos
- `ibd__list_ibd_orders`: Lista los pedidos de compra tramitados en el portal de IBD Global. Parámetros: `page` (integer, por defecto 1).
- `ibd__get_ibd_order_detail`: Consulta el detalle de un pedido de IBD Global con sus líneas de artículos, cantidades, subtotales y albaranes asociados.
- `ibd__download_ibd_order_pdf`: Descarga el documento oficial de pedido de IBD Global en formato PDF a `downloads/ibd/pedidos/`.

### Facturas y Albaranes
- `ibd__list_ibd_invoices`: Lista las facturas emitidas por IBD Global con sus importes, fechas de vencimiento y estados.
- `ibd__download_ibd_invoice_pdf`: Descarga la factura oficial de IBD Global en formato PDF a `downloads/ibd/facturas/`.
- `ibd__download_ibd_delivery_note_pdf`: Descarga el albarán de entrega / picking oficial en formato PDF a `downloads/ibd/albaranes/`.

### Cuenta
- `ibd__get_ibd_account_profile`: Consulta los datos de la cuenta de cliente, NIF/CIF, contacto y dirección en IBD Global.
