---
updated_at: 2026-10-02T21:25:30Z
source_downstream: mcp-visiotech
---

# Memoria Contextual MCP Visiotech

## Peculiaridades y Parámetros
- **Verificación 2FA**: Las herramientas `list_visiotech_invoices`, `list_visiotech_orders`, `get_visiotech_order_detail` y `get_visiotech_account_profile` admiten formalmente el parámetro opcional `two_factor_code`.
- **Cuenta Autenticada**: Usuario `VT8374DTC` (Visiotech Security).
- **Limitación de Sesión 2FA en Microservicio Downstream (`visiotech_mcp_sse`)**:
  - Al enviar los códigos OTP generados por SMS (ej. `148695`, `651864`), el endpoint downstream no persiste la sesión intermedia entre el envío de credenciales y la validación del OTP.
  - Cada invocación genera una nueva sesión HTTP independiente con un token CSRF distinto (`profile_fields`), por lo que la petición no completa el ciclo 2FA y la web redirige al formulario de login inicial.
  - Como consecuencia, `get_visiotech_product` continúa devolviendo `pvp: null`, `net_price: null` y `stock: ""`.
- **Catálogo Público Algolia**: `search_visiotech_products` opera al 100% sin autenticación para modelos, marcas, URLs y fichas S3.
- **Fichas Técnicas PDF (S3)**: Formato directo accesible `https://s3.eu-west-1.amazonaws.com/files.visiotech.es/files/pdf/<SKU>_ES.pdf`.
