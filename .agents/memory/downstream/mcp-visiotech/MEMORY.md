---
updated_at: 2026-10-03T19:12:00Z
source_downstream: mcp-visiotech
---

# Memoria Contextual MCP Visiotech

## 1. Conexión y Autenticación
- **Cuenta Autenticada**: Usuario `VT8374DTC` (Visiotech Security).
- **Verificación 2FA Oficial**:
  - `visiotech__get_visiotech_product` y tools de facturas/pedidos admiten formalmente el parámetro `two_factor_code`.
  - Cuando se invoca sin sesión activa o tras caducidad, el microservicio downstream genera una solicitud de autenticación y emite un código OTP a `dcelorrio@satyatec.es`.
  - Retorno explícito: `REQUERIDO_2FA: Visiotech requiere código de autenticación 2FA enviado a dcelorrio@satyatec.es. Por favor, vuelva a invocar la herramienta pasando el parámetro 'two_factor_code'`.
  - Tras validar 2FA, la sesión queda abierta y permite extracción en tiempo real de `pvp`, `net_price` y `stock`.

## 2. Catálogo y Búsqueda Algolia
- `visiotech__search_visiotech_products` opera contra el índice público Algolia: `sku`, `reference`, `brand`, `ean`, `discontinued`, `url`, `datasheet_url`.
- **Fichas Técnicas PDF (S3)**: Formato directo `https://s3.eu-west-1.amazonaws.com/files.visiotech.es/files/pdf/<SKU>_ES.pdf`.

## 3. Productos Clave Descubiertos
- **CCTV IP 4K (8 Megapixel):**
  - **Hikvision Domo:** `DS-2CD2183G2-LIS2U(2.8mm)` - Gama Pro AcuSense luz dual 30m.
  - **Hikvision Bullet:** `DS-2CD2683G2-LIZS2U/SRB(2.8-12mm)` - Gama Pro Varifocal luz dual/policial.
  - **Safire Smart Bullet:** `SF-IPB380A-8E1-NIGHTPRO` - AI-ISP Gama E1 8MP.
  - **Safire Smart Turret:** `SF-IPT020A-8E1-NIGHTPRO` - AI-ISP Gama E1 8MP.
- **Intrusión AJAX (Descuento B2B ~55% sobre PVP):**
  - **Central:** `AJ-HUB2PLUS-W` (PVP: 473,80 € | Neto: 213,21 €)
  - **PIR Cámara:** `AJ-MOTIONCAM-HDR-W` (PVP: 174,79 € | Neto: 78,65 €)
  - **PIR Cámara PhOD:** `AJ-MOTIONCAM-HDR-PHOD-W` (PVP: 195,60 € | Neto: 88,02 €)
  - **PIR Volumétrico:** `AJ-MOTIONPROTECT-W` (PVP: 79,07 € | Neto: 35,58 €)
  - **Magnético:** `AJ-DOORPROTECT-W` (PVP: 45,78 € | Neto: 20,60 €)
  - **Teclado con Sirena:** `AJ-KEYPADCOMBI-W` (PVP: 141,50 € | Neto: 63,68 €)
- **Detección de Incendio Óptica Convencional:**
  - **DMTECH Óptico Convencional:** `DMT-D9000-SR-V2`
  - **WizMart Óptico Convencional:** `NB-338-2-LED`
