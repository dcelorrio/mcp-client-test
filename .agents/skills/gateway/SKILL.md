---
name: gateway
description: Guía operativa completa del MCP Gateway Hub y uso de meta-herramientas.
---

# Skill MCP Gateway Hub

Esta skill define el uso del Gateway MCP central y el contrato operativo con los servidores downstream.

## Meta-Herramientas del Gateway

### 1. Búsqueda y Descubrimiento
- `gateway_search_tools`: Búsqueda de herramientas por palabras clave.
  - Parámetros: `query` (string, obligatorio), `limit` (int, opcional), `provider` (string, opcional).
- `gateway_get_tool_schema`: Consulta el esquema JSON de una herramienta downstream específica.
  - Parámetros: `action_name` (string, obligatorio).

### 2. Ejecución Downstream
- `gateway_execute_tool`: Ejecuta una acción en un servidor downstream.
  - Parámetros: `action_name` (string, obligatorio), `parameters` (objeto clave-valor con los argumentos esperados por la acción).

### 3. Ciclo de Vida de Skills y Anuncios
- `gateway_get_gateway_skill`: Devuelve la versión actual de la skill del Gateway.
- `gateway_get_announcements`: Consulta avisos, cambios de versión y novedades del Gateway (`mark_as_read: true/false`).
- `gateway_refresh_catalog`: Fuerza la sincronización y refresco del catálogo de servidores downstream.
- `gateway_propose_skill_update`: Propone mejoras a una skill.
- `gateway_list_skill_proposals`: Lista propuestas de skills existentes.
- `gateway_get_skill_proposal`: Consulta el detalle de una propuesta.
- `gateway_review_skill_proposal`: Aprueba o rechaza propuestas de skill.

## 4. Inspección de Esquemas BBDD (`db__schema_information`)
- **Uso Exclusivo**: Utilizar `db__schema_information` únicamente para obtener metadata de columnas, tipos y claves por nombre de tabla (`connection="planner_pg"|"beta10"`, `table_name="nombre"`).
- **No Enviar SQL**: Prohibido pasar sentencias `SELECT` o `JOIN` a esta herramienta. Para consultas arbitrarias usa **siempre** `db__run_query`.

## 5. Seguridad y Sanitización SQL (`db__run_query`)
- **Sanitización de Entradas**: Prohibido concatenar directamente entradas de usuario sin validar o caracteres no escapados en la cadena `sql`.
- **Restricción de Operaciones Destructivas**: Consultas de modificación/borrado masivo (`DELETE`, `UPDATE`) deben incluir cláusula `WHERE` explícita por ID. Prohibidas sentencias `DROP` o `TRUNCATE`.
- **Escapado de Comillas**: Al incrustar cadenas de texto en sentencias SQL dentro del objeto JSON, utilizar escape estándar `\'` o comillas simples dobles `''` para evitar romper el payload JSON.
- **Alias de Columna Obligatorios**: Usar alias explícitos de tabla (`t.id`, `r.name`) en consultas con JOINs para prevenir ambigüedad de columnas.


## 6. Flujo Operativo Recomendado: Inspección Previa de Conector BBDD
1. **Fase 1 (Verificación)**: Consultar la skill local o esquema del conector (`mcp-db-planner` / `mcp-db-beta10`).
2. **Fase 2 (Ejecución)**: Invocar `db__run_query` con el identificador de conexión validado (`planner_pg` o `beta10`).
3. **Protocolo de Error SQL**: Si `db__run_query` falla con columna/tabla no encontrada, invocar `db__schema_information(connection, table_name)` para actualizar la estructura local en lugar de adivinar nombres de campos.
4. **Simplificación de JOINs**: Si una consulta involucra $\ge 3$ tablas o subconsultas anidadas complejas, verificar previamente la existencia de claves en `db__schema_information` o dividir la lógica en 2 consultas secuenciales simples.
5. **Consulta Muestra Rápida**: Para verificar valores reales de registros en una tabla conocida, ejecutar `db__run_query` restringido a 1 fila (`LIMIT 1` o `FETCH FIRST 1 ROWS ONLY`). Reservar `db__schema_information` para cuando se requiera el árbol completo de relaciones/claves.






