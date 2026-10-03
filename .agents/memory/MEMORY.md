# Índice de Memoria Contextual

Este archivo actúa como índice global y contexto general del cliente MCP.

## Estructura Modular de Memoria
- **Gateway Central:** `.agents/memory/gateway/MEMORY.md` (logs globales, configuraciones de ruteo).
- **MCPs Downstream:**
  - `.agents/memory/downstream/mcp-ibd/MEMORY.md`: Pedidos y facturas de IBD Global Spain.
  - `.agents/memory/downstream/mcp-saltoki/MEMORY.md`: Caché de categorías (`3591`, `4826`) y aparellaje Hager.
  - `.agents/memory/downstream/mcp-db-beta10/MEMORY.md`: Esquemas de tablas de base de datos (`SATYA.*`).
  - `.agents/memory/downstream/mcp-visiotech/MEMORY.md`: Parámetros y peculiaridades (autenticación 2FA).
  - `.agents/memory/downstream/mcp-casmar/MEMORY.md`: Facturación 2FA re-autenticada y pedidos 2026.
  - `.agents/memory/downstream/mcp-db-planner/MEMORY.md`: Tablas de PostgreSQL Planner (`planner_pg`).
  - `.agents/memory/downstream/mcp-detnov/MEMORY.md`: Facturación y pedidos 2026 de Detnov Security.
  - `.agents/memory/downstream/mcp-aql/MEMORY.md`: Estado B2B de pedidos y facturación de AQL Protección.
  - `.agents/memory/downstream/mcp-ajax/MEMORY.md`: Espacios, hubs y dispositivos de Ajax Systems Security.
