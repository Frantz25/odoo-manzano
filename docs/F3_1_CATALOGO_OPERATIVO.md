# F3.1 — Catálogo Operativo Base

## Objetivo
Definir un catálogo estable para portal+booking con sincronización idempotente y trazable.

## Modelo mínimo
- `catalog.item`
  - `external_ref` (único, idempotencia)
  - `name`
  - `category`
  - `unit_type`
  - `capacity_units`
  - `price_base`
  - `currency`
  - `active`
  - `source_hash`
  - `source_name`
  - `last_sync_at`

## Reglas
1) `external_ref` manda para upsert.
2) Si `source_hash` no cambia, no reescribir.
3) Nunca borrar duro en sync automático; desactivar (`active=false`).
4) Log por corrida: creados/actualizados/skipped/error.

## Estrategia de sync (v1)
- Entrada: CSV versionado (`catalog_v1.csv`)
- Proceso:
  1. parse + validar columnas obligatorias
  2. calcular hash por fila
  3. upsert por `external_ref`
  4. resumir métricas

## Criterios de aceptación
- [ ] Carga inicial reproducible en entorno limpio
- [ ] Segunda corrida sin cambios => `skipped` predominante
- [ ] Cambio en 1 ítem => solo 1 update
- [ ] Reporte de sync disponible para auditoría
