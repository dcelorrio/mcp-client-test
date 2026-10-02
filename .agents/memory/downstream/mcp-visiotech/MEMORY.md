---
updated_at: 2026-10-02T22:03:40Z
source_downstream: mcp-visiotech
---

# Memoria Contextual MCP Visiotech

## Peculiaridades y Parámetros
- **Verificación 2FA Oficial**:
  - `visiotech__get_visiotech_product` ahora admite formalmente el parámetro `two_factor_code`.
  - Cuando se invoca sin sesión activa o tras caducidad, el microservicio downstream genera una solicitud de autenticación y emite un código OTP a `dcelorrio@satyatec.es`.
  - Retorno explícito del microservicio: `REQUERIDO_2FA: Visiotech requiere código de autenticación 2FA enviado a dcelorrio@satyatec.es. Por favor, vuelva a invocar la herramienta pasando el parámetro 'two_factor_code'`.
  - Al recibir el código 2FA, se debe invocar `visiotech__get_visiotech_product(sku="<SKU>", two_factor_code="<CODIGO>")`.
- **Cuenta Autenticada**: Usuario `VT8374DTC` (Visiotech Security).
- **Catálogo Público Algolia**: `search_visiotech_products` opera sin autenticación para modelos, marcas, URLs y fichas S3.
- **Fichas Técnicas PDF (S3)**: Formato directo accesible `https://s3.eu-west-1.amazonaws.com/files.visiotech.es/files/pdf/<SKU>_ES.pdf`.
