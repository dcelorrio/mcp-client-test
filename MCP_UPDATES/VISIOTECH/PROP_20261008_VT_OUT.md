# Propuesta de Actualización MCP: Visiotech

- **Proposal ID**: `PROP-20261008-111759-visi`
- **Estado**: `PENDING` (Registrado en MCP Gateway)
- **Fecha**: 08/10/2026
- **Provider**: `visiotech`
- **Herramienta**: `visiotech__get_visiotech_order_detail`

---

## Motivo y Diagnóstico

En pedidos de Visiotech con estado **`Parcialmente procesado`**, el parser actual del conector scraping solo extrae la tabla resumen general de artículos del pedido. Omite los bloques HTML inferiores correspondientes a los **Albaranes de salida (`VT/OUT/XXXXXX`)**, impidiendo discriminar qué líneas exactas están retenidas en **`ESPERANDO DISPONIBILIDAD`** y cuáles han sido remitidas en **`ENVIADO DESDE VISIOTECH`**.

---

## Prompt de Especificación Técnica para el Gateway

```markdown
### Proposal Update: `visiotech__get_visiotech_order_detail`

**Provider:** `visiotech`
**Tool Impactada:** `visiotech__get_visiotech_order_detail`

**Motivo:**
En pedidos con estado `Parcialmente procesado`, el parser omite las secciones inferiores de albaranes de salida (`VT/OUT/XXXXXX`). Esto provoca falsos positivos al determinar qué material ha sido entregado o facturado vs retenido en almacén (`ESPERANDO DISPONIBILIDAD`).

**Target Output Schema:**
Se solicita ampliar el payload JSON añadiendo la clave `delivery_notes`:

```json
{
  "provider": "visiotech",
  "title": "Pedido: SO1510342",
  "url": "https://www.visiotechsecurity.com/es/zona-de-cliente/pedidos/666885714",
  "status": "Parcialmente procesado",
  "total_lines": 5,
  "lines": [...],
  "delivery_notes": [
    {
      "delivery_note_id": "VT/OUT/1748649",
      "status": "ESPERANDO DISPONIBILIDAD",
      "items": [
        {
          "sku": "AJ-LIFEQUALITY-LITE-W",
          "description": "Ajax Wireless Temperature and Humidity monitor",
          "quantity": 16
        }
      ]
    },
    {
      "delivery_note_id": "VT/OUT/1748130",
      "status": "ENVIADO DESDE VISIOTECH",
      "tracking_url": "https://...",
      "items": [
        { "sku": "BATT-1272-U", "quantity": 60 },
        { "sku": "BATT-CR2", "quantity": 20 },
        { "sku": "RG-ES220GS-LP", "quantity": 1 }
      ]
    }
  ]
}
```

**Selectores HTML a capturar:**
- Bloque contenedor: `div.Albaranes` o secciones con encabezado `VT/OUT/`
- Badge de estado: `.badge` o `span` adyacente a `VT/OUT/` (`ESPERANDO DISPONIBILIDAD`, `ENVIADO DESDE VISIOTECH`)
- Tabla interna de productos por albarán (`sku`, `descripcion`, `cantidad`).
```
