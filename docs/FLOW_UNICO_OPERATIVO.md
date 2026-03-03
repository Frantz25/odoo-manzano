# Flujo Operativo Único — Odoo Manzano (Fase 1)

## Objetivo
Definir un flujo único, realista y verificable para evitar estados inconsistentes y bloqueos operativos.

## Flujo oficial
1. **Cotización (draft/sent)**
   - Se registra cliente, fechas, participantes y propuesta comercial.
2. **Aceptación cliente (portal)**
   - Debe aceptar políticas explícitamente.
   - Se valida disponibilidad en ese momento.
3. **Reserva tentativa (soft-hold con TTL)**
   - Se crea reserva temporal sin confirmar venta final.
   - Si vence TTL sin abono, se libera automáticamente.
4. **Abono mínimo (50%)**
   - Vía flujo estándar Odoo (anticipo/factura/pago).
5. **Confirmación definitiva (atómica)**
   - Revalida disponibilidad + abono + políticas.
   - Confirma `sale.order` y `booking` en la misma transición.
6. **Documentos y firma**
   - Generación documental trazable.
   - Firma (interna o portal) según tipo documento.
7. **Comunicaciones**
   - Eventos únicos, sin duplicidad.
   - Cliente recibe mínimo necesario; interno recibe detalle operativo.

## Política QR (obligatoria)
- En soft-hold se puede generar QR provisional vinculado a TTL.
- Solo el QR de reserva **confirmada** habilita check-in definitivo.
- Si reserva expira/cancela, QR queda inválido automáticamente.
- En confirmación final se asegura token/estado QR consistente.
- El check-in debe registrar trazabilidad (timestamp + resultado + reserva).

## Reglas duras de bloqueo
- No confirmar venta sin reserva válida.
- No confirmar reserva definitiva sin abono mínimo.
- No aceptar portal si no hay disponibilidad.
- No dejar estados `sale.order` y `booking` desincronizados.

## Criterios de aceptación Fase 1
- [ ] Flujo único aprobado por negocio.
- [ ] Reglas de bloqueo explícitas y entendibles.
- [ ] Matriz de estados definida (order ↔ booking).
- [ ] 3 escenarios QA definidos para validar el flujo antes de construir.

## Escenarios QA mínimos
1. **Happy path**: cotización → aceptación → soft-hold → abono 50% → confirmación → documento/comunicación.
2. **Sin disponibilidad**: rechazo en portal o soft-hold inválido.
3. **Sin abono suficiente**: puede existir soft-hold, pero no confirmación final.
