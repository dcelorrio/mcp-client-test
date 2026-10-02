---
trigger: always_on
---

# Directiva de Consultas MCP, Skills, Memoria, Caché y Base de Datos

## Inicialización Obligatoria de Entorno y Skill del Gateway

Al cargar este archivo o iniciar sesión en el proyecto, el cliente debe:
1. **Verificar y garantizar la existencia de la siguiente estructura base de carpetas y archivos:**
   - `.agents/skills/gateway/`
   - `.agents/skills/downstream/`
   - `.agents/memory/gateway/`
   - `.agents/memory/downstream/`
   - `.agents/memory/MEMORY.md`
   - `.agents/cache/gateway/`
   - `.agents/cache/downstream/`

   Si alguna de estas rutas no existe localmente, créala inmediatamente.

2. **Sincronización Dinámica de la Skill del Gateway:**
   - Comprobar si existe `.agents/skills/gateway/SKILL.md`. Si no existe, invocar la tool `gateway_get_gateway_skill` y persistir la información en esa ruta.
   - Si se reciben avisos de cambios mediante `gateway_get_announcements` o tras ejecutar `gateway_refresh_catalog`, re-ejecutar `gateway_get_gateway_skill` para sobrescribir y mantener actualizada la skill local.

## Arquitectura Estructurada del Cliente

```text
.
└── .agents/
    ├── rules/
    │   └── GEMINI.md                    # Reglas globales del cliente (Ruteo gateway, descubrimiento, post-inferencia)
    │
    ├── skills/
    │   ├── gateway/
    │   │   └── SKILL.md                 # Skill del MCP Gateway central (uso de gateway_execute_tool, etc.)
    │   │
    │   └── downstream/                  # Catálogo modular por MCP downstream
    │       ├── mcp-visio/
    │       │   ├── SKILL.md             # Guías operativas y flujos de uso funcional
    │       │   └── tools.json           # Manifiesto formal con los esquemas JSON de las herramientas
    │       ├── mcp-ibd/
    │       │   ├── SKILL.md
    │       │   └── tools.json
    │       └── <nombre-mcp-downstream>/
    │           ├── SKILL.md
    │           └── tools.json
    │
    ├── memory/                          # Sistema de Memoria Contextual Persistente (Estructural)
    │   ├── MEMORY.md                    # Índice global / Registro general de sesiones
    │   │
    │   ├── gateway/
    │   │   └── MEMORY.md                # Memoria contextual propia del MCP Gateway central
    │   │
    │   └── downstream/                  # Memorias aisladas por MCP (Esquemas BBDD, reglas, directivas de uso)
    │       ├── mcp-visio/
    │       │   └── MEMORY.md
    │       ├── mcp-db-crm/
    │       │   └── MEMORY.md
    │       └── <nombre-mcp-downstream>/
    │           └── MEMORY.md
    │
    └── cache/                           # Sistema de Caché Temporal de Consultas (Volátil)
        ├── gateway/                     # Respuestas y auditorías temporales del Gateway
        │
        └── downstream/                  # Caché aislada por downstream (Resultados puntuales, TTL)
            ├── mcp-ibd/
            │   └── <consulta>.md        # Resultados de consultas con timestamp y TTL
            └── <nombre-mcp-downstream>/
                └── ...
```

## Reglas y Directivas Obligatorias

1. **Ruteo de Consultas Exclusivo vía `mcp-gateway`:**
   - En este proyecto, cualquier consulta, inspección de esquemas, búsqueda de herramientas, datos o interacción con proveedores debe realizarse **ÚNICA Y EXCLUSIVAMENTE** a través de la herramienta `mcp-gateway` (`gateway_execute_tool`, `gateway_search_tools`, `gateway_get_tool_schema`, etc.).
   - Queda strictly prohibido acceder, leer o analizar archivos de código de servidores o scripts locales (como archivos `.py`) para diagnosticar o usar herramientas. Toda interacción se limita al contrato expuesto por `mcp-gateway`.

2. **Descubrimiento Progresivo de Tools y Skills por MCP Downstream:**
   - Al interactuar por primera vez con un MCP *downstream* específico a través del gateway:
     a) **Manifiesto Formal de Tools (`tools.json`):**
        - Comprobar si existe `.agents/skills/downstream/<nombre-mcp>/tools.json`.
        - Si no existe: Solicitar al `mcp-gateway` el listado de herramientas de ese downstream con sus esquemas JSON correspondientes (`gateway_search_tools` / `gateway_get_tool_schema`) y guardarlo en dicho archivo.
        - En ejecuciones posteriores, consultar directamente este archivo para conocer los parámetros exactos sin realizar peticiones redundantes de esquemas al gateway.
     b) **Guía Operativa (`SKILL.md`):**
        - Comprobar si existe `.agents/skills/downstream/<nombre-mcp>/SKILL.md`.
        - Si no existe: Sintetizar las pautas de uso, flujos habituales, combinaciones de herramientas y particularidades operativas, persistiendo la guía en esa ruta.

3. **Estructura Aislada de Memoria Contextual (Gateway y Downstream):**
   - La memoria del proyecto debe modularizarse en la carpeta `.agents/memory/`.
   - El **Gateway central** dispone de su propia memoria contextual en `.agents/memory/gateway/MEMORY.md`.
   - Cada MCP **downstream** o proveedor debe tener su propio espacio de memoria aislado en `.agents/memory/downstream/<nombre-mcp>/MEMORY.md` para evitar mezclar contextos entre distintas bases de datos o servicios.
   - Existe un `.agents/memory/MEMORY.md` raíz solo para el índice general y contexto global de la aplicación.

4. **Cero Suposiciones de Esquema de Base de Datos:**
   - Queda strictly prohibido adivinar o asumir nombres de columnas en tablas de base de datos.
   - Antes de ejecutar una consulta sobre una tabla desconocida o no verificada previamente, se debe consultar su estructura mediante las herramientas de esquema del `mcp-gateway` (o revisar la memoria del MCP *downstream* correspondiente en `.agents/memory/downstream/<nombre-mcp>/MEMORY.md`).
   - Las estructuras de columnas descubiertas deben registrarse inmediatamente en la memoria del MCP *downstream* afectado para su reutilización futura.

5. **Criterio Universal de Persistencia y Marcas de Tiempo (Memoria vs. Caché):**
   Al finalizar cada interacción a través del gateway, el agente debe evaluar la naturaleza de la información obtenida bajo un criterio estricto de bifurcación:

   a) **Destino Memoria Contextual (`.agents/memory/` — Conocimiento Estructural y Permanente):**
      - **Criterio:** Conocimiento duradero necesario para operar con precisión en el futuro.
      - **Aplica a:** Esquemas de bases de datos, directivas/peculiaridades de invocación descubiertas (tokens obligatorios, 2FA, filtros compuestos), relaciones lógicas y constantes de negocio.
      - **Control Temporal:** Toda entrada o actualización debe incorporar cabecera YAML con metadatos:
        ```yaml
        ---
        updated_at: YYYY-MM-DDTHH:mm:ssZ
        source_downstream: <nombre-mcp>
        ---
        ```
      - Si se crea un nuevo MCP downstream por primera vez en memoria, añade su enlace y descripción corta en el índice global `.agents/memory/MEMORY.md`.

   b) **Destino Caché Temporal (`.agents/cache/` — Datos de Estado Transitorios):**
      - **Criterio:** Resultados puntuales de consultas y ejecuciones que reflejan el estado vivo del sistema y están sujetos a cambio u obsolescencia.
      - **Aplica a:** Listados de pedidos, transacciones, albaranes, facturas, stock puntual, balances o cálculos temporales.
      - **Control Temporal y Expiración (TTL):** Toda entrada en caché debe incorporar obligatoriamente cabecera YAML:
        ```yaml
        ---
        cached_at: YYYY-MM-DDTHH:mm:ssZ
        ttl_hours: 24
        source_downstream: <nombre-mcp>
        ---
        ```
      - Antes de reutilizar una entrada de caché, el agente verificará si `now - cached_at > ttl_hours`. Si ha expirado, debe invalidar el archivo y consultar datos frescos al gateway.

6. **Resumen Obligatorio de Ciclos de Inferencia:**
   Al finalizar cualquier respuesta o tarea, incluir un resumen detallado de los ciclos de inferencia utilizando la siguiente simbología, tipificación y desglose dual de tiempos (reloj real vs downstream):
   - **Simbología de Estado:**
     - 🟢 **Punto verde:** Ciclo ejecutado de manera sintácticamente correcta y sin errores.
     - 🔴 **Punto rojo:** Ciclo que produjo un error sintáctico, de argumentos o de ejecución.
     - 🔵 **Punto azul:** Ciclo donde se obtuvo una ventaja explícita gracias al uso previo de una skill, memoria (`.agents/memory/`) o regla guardada.
   - **Taxonomía de Tipos de Ciclo:**
     - `[DISCOVERY]`: Inspección de catálogo, esquemas BBDD o manifiestos de herramientas.
     - `[EXEC]`: Ejecución de consultas directas (SQL, APIs de proveedores, comandos).
     - `[CACHE/MEM]`: Reutilización directa de memoria, reglas o caché existente.
     - `[PERSIST]`: Escritura o actualización en `.agents/memory/` o `.agents/cache/`.
     - `[RETRY]`: Corrección de errores sintácticos o de argumentos tras fallo.
     - `[ORCHEST]`: Planificación, desglose de subagentes o agregación multi-fuente.
   - **Formato por Ciclo (Desglose Dual):**
     `- <Símbolo> Ciclo N [<TIPO>] (~X.Xs real | ~Y.Ys tool): Descripción concisa de la acción.`
     *(Donde `real` es el tiempo total de reloj del ciclo [pensamiento LLM + red + ejecución] y `tool` es el tiempo neto del servidor downstream, BBDD o disco).*
   - **Métricas Globales:**
     - ⏱️ **Tiempo Real Total (Wall-Clock):** Duración real en segundos desde que el usuario envía el mensaje hasta que se entrega la respuesta.
     - ⏱️ **Tiempo Neto Downstream (Tools/BBDD):** Tiempo neto empleado por queries, APIs o escrituras en disco.
     - 📊 **Estimación de Ventana de Contexto:** Tamaño aproximado de la ventana de contexto utilizada en ese momento.

7. **Rigor de Inferencia y Temperatura por Categoría de MCP:**
   Al interactuar con los diferentes MCPs *downstream*, el agente debe ajustar su modo de razonamiento y rigidez según la naturaleza del servicio:
   - **Portales de Proveedores B2B (`ibd`, `saltoki`, `visiotech`, `casmar`, `detnov`, `aql`):**
     - *Modo:* Determinismo absoluto (Temp ≈ 0.1).
     - *Criterio:* Cero alucinación o redondeo en importes, fechas, SKUs o estados de pedido. Extracción y presentación literal de datos. Parámetros de tools verificados contra su `tools.json`.
   - **Bases de Datos Relacionales / ERP (`db-beta10`, `db-planner`):**
     - *Modo:* Precisión de esquema y lógica relacional (Temp ≈ 0.2).
     - *Criterio:* Construcción estricta de consultas SQL respetando tipos y nombres exactos de columnas registrados en memoria. Flexibilidad analítica solo para estructurar `JOINs` y relaciones lógicas entre tablas conocidas.
   - **Dispositivos y Seguridad Física (`ajax`):**
     - *Modo:* Telemetría en tiempo real (Temp ≈ 0.1).
     - *Criterio:* Notificación precisa y fidedigna de estados de hubs, zonas, sensores y eventos, sin inferencias especulativas.
   - **Búsqueda Semántica / Vectorial (`qdrant`, `synology`):**
     - *Modo:* Flexibilidad contextual (Temp ≈ 0.3 - 0.4).
     - *Criterio:* Interpretación de lenguaje natural, similitud semántica y búsqueda conceptual antes de filtrar o condensar los resultados.

8. **Paralelización de Tareas Independientes (Subagentes y Concurrencia):**
   - **Criterio de Activación:** El agente principal debe evaluar si la petición del usuario contiene dos o más operaciones independientes (sin dependencia secuencial estricta). Aplica a:
     a) **Inter-MCP:** Consultas simultáneas a múltiples MCPs *downstream* (ej. comparar disponibilidad o facturación entre varios proveedores o cruzar proveedores con ERP).
     b) **Intra-MCP:** Consultas concurrentes dentro de un mismo MCP (ej. solicitar pedidos y facturas a la vez, consultar lotes de productos o paginación pesada).
   - **Rol del Orquestador:** El agente principal actúa como coordinador:
     - Dispara las tareas en paralelo mediante `invoke_subagent` (un subagente por fuente o tarea independiente).
     - Cada subagente consulta su `tools.json` y ejecuta sus llamadas al gateway de forma aislada.
   - **Consolidación (Reduce):** El agente principal recibe las respuestas procesadas, unifica los datos en una respuesta final coherente (tablas comparativas, auditorías) y gestiona la persistencia en `.agents/cache/` si aplica.

