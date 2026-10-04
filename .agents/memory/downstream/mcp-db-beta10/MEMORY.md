---
updated_at: 2026-10-04T10:16:00Z
source_downstream: mcp-db-beta10
---

# Memoria Contextual MCP DB Beta10

## Esquemas de BBDD (`beta10`)
- **`SATYA.EMPRESA`**: `IDEMPRESA`, `NOMBRE`
  - *Empresas*: 1=SATYA, 4=INERTYA, 7=NAVYA, 8=INVARYA.
- **`SATYA.FACTURACLI`**: `IDFACTURACLI`, `FFACTURA`, `NFACTURA`, `IDSERIE_FACTURACLI`, `ESTADO`, `IDCLIENTE`, etc.
- **`SATYA.LFACTURACLI`**: `IDLFACTURACLI`, `IDFACTURACLI`, `IMPORTE`, etc.
- **`SATYA.SERIE_FACTURACLI`**: `IDSERIE_FACTURACLI`, `IDEMPRESA`, `DESCRIPCION`
- **`SATYA.FACTURAPRO`**: `IDFACTURAPRO`, `IDPROVEEDOR`, `FFACTURAPRO`, `FREGISTRO`, `NFACTURAPRO`, `NFAC_PROVEEDOR`, `IDSERIE_FACTURAPRO`, `IDEST_FACTURAPRO`, `RAZON_SOCIAL`, `CIF`, `BASE_IMPONIBLE`, `IVA`, `RECARGO`, `RETENCION`, `TOTAL`, `IMPORTE`.
- **`SATYA.LFACTURAPRO`**: `IDLFACTURAPRO`, `IDFACTURAPRO`, `IDARTICULO`, `DESCRIPCION`, `UNIDADES`, `PRECIO`, `DTO`, `IVA`, `IMPORTE`.
- **`SATYA.SERIE_FACTURAPRO`**: `IDSERIE_FACTURAPRO`, `IDEMPRESA`, `DESCRIPCION`, `DESC_CORTA`, `ANIO`.
- **`SATYA.ARTICULO`**: `IDARTICULO`, `CODIGO`, `DESCRIPCION`, `PRECIOULTCOMPRA`, `FECHAULTCOMPRA`, `IDFABRICANTE`, `ESTADO`.
- **`SATYA.TARIFA`**: `IDTARIFA`, `IDARTICULO`, `PRECIO`, `MARGEN`, `ESTADO`.
- **`SATYA.PROVEEDOR`**: `IDPROVEEDOR`, `NOMBRE`, `NOMBRE_COMERCIAL`, `CIF`.
- **`SATYA.FABRICANTE`**: `IDFABRICANTE`, `DESCRIPCION`.
- **`SATYA.EMPLEADO`**: `IDEMPLEADO`, `NOMBRE_Y_APELLIDO`, `ESTADO`.
  - *Técnicos clave*: 216=Andrés Méndez Fernández, 233=Víctor Tenas Jiménez.
- **`SATYA.TIEMPO_TRABAJADO`**: `IDTIEMPO_TRABAJADO`, `IDPARTE_MONTAJE`, `IDEMPLEADO`, `FINICIO`, `FFIN`, `HORAS`, `ES_DESPLAZAMIENTO`, `ESTADO`. (Tabla oficial de imputación de horas de mano de obra y desplazamientos en partes de trabajo).
- **`SATYA.ORDEN_TRABAJO`**: `IDORDEN_TRABAJO`, `NORDEN_TRABAJO`, `IDCLIENTE`, `IDSISTEMA`, `IDTACTUACION_SISTEMA`, `DURACION_ESTIMADA` (horas; en preventivos suele venir a 0 por defecto), `ESTADO`.
- **`SATYA.ORDEN_TRABAJO_MANT`**: `IDORDEN_TRABAJO_MANT`, `IDORDEN_TRABAJO`, `IDSISTEMA_MANT`. (Tabla pivote vinculante entre la OT generada y las revisiones contratadas del sistema).
- **`SATYA.SISTEMA_MANT`**: `IDSISTEMA_MANT`, `IDSISTEMA`, `IDTACTUACION`, `IDTSUBSIS`, `DURACION_ESTIMADA` (en minutos; tiempo teórico de referencia contractual), `ESTADO`.
- **`SATYA.TACTUACION`**: `IDTACTUACION`, `DESCRIPCION`, `TIPO` (1=Instalación/Obra/Correctivo, 2=Revisión/Mantenimiento, 3=Avería).
- **`SATYA.SISTEMA_CUOTA` / `SATYA.CONTRATO_CUOTA`**: `IDCONTRATO`, `PRECIO_MES`, `DTO`, `UNIDADES`, `FCONTRATACION`, `ESTADO`. (Cuotas de mantenimiento contratadas).
- **`SATYA.PARTE_MONTAJE`**: `IDPARTE_MONTAJE`, `IDORDEN_TRABAJO`, `FMONTAJE`, `ESTADO`.

## Packages Oficiales de Cálculo y Escritura (`SATYA`)
- **Lectura total horas OT**: `SATYA.PKG_ORDEN_TRABAJO.TotalTiempoOrdenTrabajo(IDORDEN_TRABAJO)`
- **Escritura oficial de tiempos teóricos preventivos**: `SATYA.PKG_SISTEMA_MAN.MAN_SISTEMA_MANT`

## Particularidad de Contratos Cabecera vs Sistemas Técnicos (Cuota Cero)
- **Modelado en Cuentas de Infraestructura** (`UTE MALEBU`, `COBRA / ENDESA`, `AYTO. MEDIANA`, `UTE AVE ENERGIA`):
  - Existe un **Sistema Cabecera** (`2423.1 CONTRATO CABECERA...`, etc.) donde reside el contrato global y el 100% de la cuota en `SISTEMA_CUOTA` / `CONTRATO_CUOTA`.
  - Existen múltiples **Sistemas Técnicos Hijos** (subestaciones, centros de transformación, túneles, edificios) donde `SISTEMA_CUOTA` es NULL o 0, pero tienen `SISTEMA_MANT.ACTIVAR = 1` y generan las OTs preventivas periódicas individuales.
  - Para análisis de rentabilidad / cuota por OT: un `JOIN` directo `SISTEMA -> SISTEMA_CUOTA` devuelve cuota 0 €. Se requiere prorratear la cuota global del contrato/cliente entre las OTs o revisiones activas del periodo.
