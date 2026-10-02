---
updated_at: 2026-10-02T21:19:00Z
source_downstream: visiotech
---

# Memoria Operativa Downstream - Visiotech Security

## 1. Conexión y Estado de Sesión
- **Usuario configurado:** `VT8374DTC`
- **Comportamiento de Autenticación:** La consulta de productos (`visiotech__get_visiotech_product`) depende de una sesión activa de instalador B2B. Si la sesión en el servidor microservicio backend no está validada/autenticada, las peticiones HTTP a la web son redirigidas a la pantalla de login (`return=aHR0cHM...`), por lo que los campos `pvp`, `net_price` y `stock` se devuelven como `null` / vacíos y la descripción contiene el texto residual de pie de página ("Te ayudamos con FAQs, Tutoriales y Software disponibles").

## 2. Catálogo y Búsqueda Algolia
- `visiotech__search_visiotech_products` opera contra el índice público Algolia y devuelve con precisión: `sku`, `reference`, `brand`, `ean`, `discontinued`, `url`, `datasheet_url`.
- Fichas técnicas oficiales en PDF se resuelven en S3: `https://s3.eu-west-1.amazonaws.com/files.visiotech.es/files/pdf/<SKU>_ES.pdf`.

## 3. Productos Clave Descubiertos
- **CCTV IP 4K (8 Megapixel):**
  - **Hikvision Domo:** `DS-2CD2183G2-LIS2U(2.8mm)` - Gama Pro AcuSense luz dual 30m.
  - **Hikvision Bullet:** `DS-2CD2683G2-LIZS2U/SRB(2.8-12mm)` - Gama Pro Varifocal luz dual/policial.
  - **Safire Smart Bullet:** `SF-IPB380A-8E1-NIGHTPRO` - AI-ISP Gama E1 8MP.
  - **Safire Smart Turret:** `SF-IPT020A-8E1-NIGHTPRO` - AI-ISP Gama E1 8MP.
- **Detección de Incendio Óptica Convencional:**
  - **DMTECH Óptico Convencional:** `DMT-D9000-SR-V2`
  - **WizMart Óptico Convencional:** `NB-338-2-LED`
