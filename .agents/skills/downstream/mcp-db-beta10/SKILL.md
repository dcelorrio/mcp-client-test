---
name: mcp-db-beta10
description: Guía de ruteo y consultas SQL para el ERP Oracle SATYA (beta10) vía db_gateway.
---

# Skill: DB Gateway — Oracle ERP Beta10 (`beta10`)

> [!IMPORTANT]
> **CONEXIÓN VÍA DB GATEWAY**
> Para consultar la base de datos de gestión corporativa SATYA (Oracle), usa **siempre** la herramienta `db__run_query` con `connection="beta10"`.
> ```python
> db__run_query(connection="beta10", sql="SELECT * FROM SATYA.EMPRESA")
> ```

## Reglas de Sintaxis SQL (Oracle)
- Los nombres de tablas deben llevar el esquema: `SATYA.EMPRESA`, `SATYA.FACTURACLI`, `SATYA.ARTICULO`.
- El identificador de empresa principal de SATYA es `IDEMPRESA = 1`.
- Limitar resultados mediante `FETCH FIRST N ROWS ONLY` o `ROWNUM <= N`.

## 2. Mapeo de Albaranes de Cliente y Etiquetas (Beta10)
- **Identificador de Tabla**: `IDTABLA = 141` corresponde a la entidad `SATYA.ALBARANCLI`.
- **Consulta de Etiquetas**: Para filtrar albaranes por tags, cruzar `SATYA.ALBARANCLI` con `SATYA.TAGS_X_TABLA` mediante `IDTABLA = 141` y `ID = ALBARANCLI.IDALBARANCLI`.
- **Estados de Albarán**: Conciliar albaranes pendientes de facturación filtrando por estado de cabecera en `ALBARANCLI` antes de procesar el lote.

## 3. Verificación Previa de Contexto (Beta10 / Oracle)
- **Carga de Skill Obligatoria**: Antes de ejecutar llamadas a packages oficiales (`SATYA.PKG_SISTEMA_MAN`, `SATYA.PKG_ORDEN_TRABAJO`), verificar la presencia del esquema y parámetros en la skill local de `mcp-db-beta10`.
- **Prevención de Sentencias Vacías**: Verificar obligatoriamente que la cadena `sql` contenga una instrucción ejecutable completa (`SELECT ... FROM ...`) no vacía antes de invocar `db__run_query(connection="beta10")`.



