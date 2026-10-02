---
name: mcp-ajax
description: Habilidad operativa y de consulta para interactuar con la API REST y herramientas MCP de Ajax Systems Security (gestión de espacios, hubs, zonas/estancias, dispositivos, sensores, armados y logs de eventos).
---

# Ajax Systems MCP Assistant

Esta habilidad guía al asistente en la gestión y monitoreo de sistemas de alarma **Ajax Systems** a través de las 36 herramientas dedicadas del servidor MCP.

## Herramientas Clave de Ajax Systems
- `ajax__ajax_list_spaces()`: Lista todos los espacios comercializados/monitoreados por SATYA.
- `ajax__ajax_list_hubs()`: Lista los hubs de alarma en propiedad o monitoreo.
- `ajax__ajax_get_space_details(space_id)`: Detalle completo de un espacio.
- `ajax__ajax_list_space_devices(space_id)`: Dispositivos y detectores vinculados a un espacio.
- `ajax__ajax_get_event_logs(space_id)`: Historial de eventos y alarmas.
- `ajax__ajax_set_security_mode(space_id, mode)`: Armado / Desarmado de la alarma.
