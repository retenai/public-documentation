# Reten – Documentación de Integraciones Reales (Plan)

Este documento planifica una nueva versión de la documentación centrada en el “estado actual” de las integraciones de clientes con Reten. A diferencia de la documentación "ideal" (orientada a entidades normalizadas y procesos estándar), aquí documentaremos cómo funcionan hoy las integraciones reales, principalmente vía archivos depositados periódicamente en buckets, para dos entidades clave: Comercios y Transacciones.

## Objetivo
- Documentar de forma clara y accionable cómo integran hoy los clientes.
- Reducir fricción operativa (onboarding, soporte, troubleshooting) y asegurar coherencia entre equipos.
- Sentar bases para evolucionar gradualmente hacia el modelo ideal.

## Alcance y audiencia
- Alcance: Integraciones por archivos en buckets (ingesta batch) de Comercios y Transacciones.
- Audiencia: Ingenieros de datos, integradores técnicos de clientes, soporte/operaciones, PM/PO.

## Diferencias con la documentación "ideal"
- La ideal asume datos normalizados y contratos uniformes.
- La real describe formatos, variaciones por cliente, convenciones, y excepciones hoy vigentes.
- Mantendremos un mapeo explícito entre “Real → Ideal” para facilitar convergencia futura.

## Estructura propuesta de esta documentación (Real)

1) Introducción
- Contexto de ingesta por buckets, alcance, responsabilidades cliente/Reten.

2) Arquitectura de Integración (Actual)
- Fuentes: buckets por cliente (nombres, accesos, segregación por entorno).
- Modalidad: batch periódica (frecuencias típicas, ventanas de entrega, horarios, timezones).
- Disparadores: jobs de ingesta, validación, enriquecimiento y carga en Reten.

3) Entidades y Esquemas (Actuales)
- Comercios
  - Campos mínimos requeridos, opcionales y condicionales.
  - Tipos de datos, formatos (fechas, monedas, booleanos, IDs), encoding, separadores.
  - Variaciones por cliente (nombres de columnas, presence/absence de campos, normalización).
  - Validaciones mínimas (unicidad, referencia, formato) e idempotencia.
  - Ejemplos reales (CSV/Parquet) anonimizados.
  - Mapeo a modelo ideal (campo_real → campo_ideal) y gaps.
- Transacciones
  - Estructura, claves de unicidad, referencias a Comercios.
  - Manejo de montos, impuestos, descuentos, divisas, horas/fechas, estado.
  - Reglas de duplicados, reenvíos (reprocessing), actualizaciones tardías.
  - Ejemplos reales (CSV/Parquet) y mapeo a ideal.

4) Estándares Operativos Comunes
- Formatos soportados: CSV (delimitador, quote, decimal), Parquet; encoding (UTF-8), compresión.
- Convenciones de nombre de archivos (prefijos, sufijos de fecha, particiones) y paths en buckets.
- SLA de entrega, ventanas de corte, reintentos, política de late-arriving data.
- Idempotencia y control de duplicados (naming + claves + marcas de procesamiento).
- Timezones, locales (separador decimal, formato fecha), normalización.
- Seguridad: permisos de acceso al bucket, PII (enmascarado/anonimización), retención de datos.

5) Proceso de Ingesta y Calidad
- Pipeline: descubrimiento de archivos, validación de esquema y contenido, transformación, carga.
- Validaciones: esquema, tipos, catálogos, integridad referencial, reglas de negocio mínimas.
- Errores: clasificación (críticos/no críticos), DLQ (dead-letter), reintentos, notificaciones/alertas.
- Reconciliación: conteos, sumas de control, checks de fin de día, reportes de discrepancias.
- Backfills y re-procesos: protocolos, ventanas, resguardo y versionado de archivos.

6) Operación y Observabilidad
- Métricas clave: archivos esperados/recibidos/procesados, tasas de error, latencias.
- Dashboards/alertas (a definir), canales de soporte, procedimientos de escalamiento.
- Playbooks de troubleshooting (por error típico) y preguntas frecuentes.

7) Anexos por Cliente (Variaciones)
- Para cada cliente: 
  - Bucket y rutas, frecuencia, formatos, particularidades de campos.
  - Ejemplos propios, excepciones acordadas, SLA específicos.
  - Mapeo Real → Ideal específico del cliente.

8) Roadmap de Convergencia hacia el Modelo Ideal
- Gap analysis (por entidad y por cliente).
- Quick wins vs cambios estructurales.
- Plan de migración gradual: hitos, dependencias y riesgos.

## Plan de Trabajo (Milestones)

M0 – Planificación (este documento)
- Alinear alcance, audiencia y objetivos.

M1 – Descubrimiento y Recolección
- Levantar estado actual por cliente (buckets, rutas, formatos, ejemplo de archivos).
- Identificar variaciones y excepciones por entidad (Comercios, Transacciones).

M2 – Definición de Contratos “Reales” Base
- Proponer esquemas base por entidad (mínimos requeridos + opcionales + condicionales).
- Definir estándares operativos (formatos, nombres, particiones, timezones, seguridad).

M3 – Documentación Operativa
- Procesos de ingesta, validaciones, manejo de errores, reconciliación, backfills.
- Observabilidad: métricas, alertas y playbooks.

M4 – Anexos por Cliente
- Documentar variaciones por cliente con ejemplos anonimizados y mapeos a ideal.

M5 – Convergencia
- Gap analysis y roadmap de migración hacia la documentación/contratos ideales.

## Información que necesitaremos (por cliente)
- Bucket(s) y rutas exactas (producción y, si aplica, sandbox).
- Tipos de archivos (CSV/Parquet), compresión, encoding, delimitadores, quote.
- Frecuencia de entrega y ventana horaria; timezone.
- Convención de nombres (prefijos/sufijos/fechas/particiones) y tamaño típico de archivos.
- Esquemas actuales (nombres de columnas, tipos, catálogos) y muestra de archivos.
- Claves de unicidad, reglas de actualización, campos de referencia entre entidades.
- Tratamiento de PII y requisitos de seguridad/retención.
- Expectativas de SLA y canales de soporte.

## Próximos pasos inmediatos
1) Validar esta estructura/plan contigo.
2) Incorporar el primer cliente como anexo piloto (con ejemplos anonimizados).
3) Completar los contratos “reales” base de Comercios y Transacciones.

---

Notas:
- Esta carpeta en projects/ contiene la documentación de “estado actual”. La documentación “ideal” existente se mantendrá por ahora tal cual, y definiremos después el mecanismo para ocultarla o aislarla públicamente.
- El objetivo es publicar esta documentación interna primero; luego, cuando esté madura, expondremos las partes necesarias hacia fuera con los contratos estabilizados.