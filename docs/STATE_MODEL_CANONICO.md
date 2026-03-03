# Modelo de Estados Canónico (E2.1)

## Principio
`booking.state` es la **fuente de verdad** del estado operativo de reserva.

`sale.order` mantiene su estado comercial (`draft/sent/sale/cancel`) y refleja el estado de reserva solo como vista/control derivado.

---

## Estados canónicos de Booking
- `draft` → reserva creada pero incompleta (sin hold válido)
- `reserved` → soft-hold activo (con TTL)
- `confirmed` → reserva definitiva confirmada
- `cancelled` → reserva anulada (manual o por expiración/rollback)

---

## Mapeo order ↔ booking

| sale.order.state | booking.state esperado | Observación |
|---|---|---|
| draft/sent | draft o reserved | etapa comercial / hold |
| sale | confirmed | confirmación atómica obligatoria |
| cancel | cancelled | cancelación sincronizada |

**Regla dura:** si `sale.order.state = sale`, entonces `booking.state` debe ser `confirmed`.

---

## Transiciones permitidas (booking)
- `draft -> reserved` (soft-hold generado)
- `reserved -> confirmed` (abono + validaciones OK)
- `reserved -> cancelled` (expira TTL o cancelación)
- `confirmed -> cancelled` (cancelación operacional)

No permitidas:
- `draft -> confirmed` sin validaciones completas
- `cancelled -> confirmed` sin recreación explícita

---

## Reglas de sincronización
1. Confirmación final:
   - validar políticas + disponibilidad + abono mínimo
   - `sale.order` y `booking` actualizan en una sola transición lógica
2. Cancelación:
   - cancelar `sale.order` fuerza `booking -> cancelled`
   - liberar capacidad/unidades
3. Expiración TTL:
   - cron marca `reserved -> cancelled`
   - no permite check-in
4. Integridad:
   - job de auditoría detecta y reporta inconsistencias (`sale` con booking != confirmed)

---

## Criterios de aceptación E2.1
- Existe tabla de estados y transiciones aprobada.
- Están documentadas transiciones prohibidas.
- Hay reglas explícitas de sincronización order↔booking.
- No se permite por diseño `sale` sin booking `confirmed`.
